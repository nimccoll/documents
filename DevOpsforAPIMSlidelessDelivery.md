# DevOps for Azure API Management - slideless delivery

## Service Description

Many customers are already leveraging Azure DevOps to deploy the APIs they are hosting in Azure API Management. However, automated deployment of the API configuration information, including products and inbound and outbound policies stored in API Management, has been difficult to accomplish until the recent release of the Azure API Management DevOps Resource Kit (https://github.com/Azure/azure-api-management-devops-resource-kit). The kit provides sample ARM templates and a command-line tool that allows customers to leverage Azure Resource Manager to deploy the configuration associated with the APIs they are hosting in Azure API Management.

## Agenda

- Challenges with managing API configurations
- How the Azure API Management DevOps Resource Kit addresses the challenges
- Using an Infrastructure as Code (IaC) approach to deploy an entire API Management instance
- Introducing the APIMTemplate tool
- Creating API configuration using YAML
- Extracting API configuration from API Management
- Source control for API configuration
- Create a Build Pipeline for your API configuration
- Deploy your API configuration using a Release Pipeline

## Prerequisites

- The ability to join a Microsoft Teams call and screen share.
- Access to an Azure DevOps subscription with the ability to create an Azure DevOps build pipeline and create an Azure DevOps release pipeline.
- Access to an Azure subscription containing two API Management instances and a storage account. The first instance will act as your development environment and you will deploy your API configurations to the second instance via Azure DevOps pipelines. The storage account will be used to store the XML policy files exported from API Management. Your Azure DevOps subscription must be configured with a service connection that has permission to deploy resources to this Azure subscription.
- A simple API and associated APIM configuration that also includes at least one APIM policy.

    This will be a demonstration-led session. The customer should have access to an Azure DevOps subscription and an Azure subscription so they can follow along and build out their own POC during the delivery.  

## Challenges with Managing API Configurations

Discuss the key challenges with managing API configuration as documented here https://github.com/Azure/azure-api-management-devops-resource-kit#the-problem

- How to automate deployment of APIs into API Management?
- How to migrate configurations from one environment to another?
- How to avoid interference between different development teams who share the same API Management instance?

## How does the Azure API Management DevOps Resource Kit address the challenges?

Discuss how the DevOps Resource Kit for APIM can help to address the challenges of managing API configuration. Refer to the diagram and talking points found here https://github.com/Azure/azure-api-management-devops-resource-kit#cicd-with-api-management

## Using an Infrastructure as Code (IaC) approach to deploy an entire API Management instance

Discuss how the resource kit includes a series of ARM templates in the example folder (https://github.com/Azure/azure-api-management-devops-resource-kit/tree/master/example) that can be leveraged to deploy a new APIM instance and all of its supporting resources to Azure. The templates are broken down as follows.

- Service template contains all of the service-level configuration for the API Management instance such as pricing tier, custom domains, network configuration, etc.
- Shared templates contain shared resources such as products, subscriptions, groups, and loggers
- API templates contain API configuration and their associated resources such as operations, policies, and diagnostic settings.
- Master template can be used to link all of the previous templates together and deploy them as one unit. Each template can also be deployed individually.

Using these ARM templates you could take an Infrastructure as Code (IaC) approach and deploy an entire APIM instance and all of its associated API configuration to an Azure subscription.

## Introducing the APIMTemplate tool

While leveraging the ARM templates provided by the resource kit provides an excellent way to manage the infrastructure that makes up our APIM instance how does the toolkit address the typical developer workflow as APIs are being created and maintained during the development lifecycle?

This is where the APIMTemplate command line tool fits into the process. Talk through how the tool is made available in both source code format as well as pre-built binaries for different platforms. Show the customer where they can download the pre-built binaries (https://github.com/Azure/azure-api-management-devops-resource-kit/releases). Walk them through downloading and extracting it on the platform of their choice.

Discuss how the tool offers two modes of operation.

1. Create
1. Extract

## Creating API configuration using YAML

- Explain to the customer how the Create mode of the APIMTemplate tool relies on a YAML file to describe the API configuration. Explain how this works well for developers who aren't using the Azure portal or a Visual Studio or Visual Studio Code extension to manage their API configurations in a development APIM instance. Show the complete YAML configuration file here https://github.com/Azure/azure-api-management-devops-resource-kit/tree/master/src/APIM_ARMTemplate#sample-config-file and discuss the different properties that make up the file.
- After reviewing the full configuration explain that you can create a simple YAML configuration with a minimal number of properties to describe your API configuration. Show the simple YAML configuration example found here https://nimccoll.visualstudio.com/GitDemo/_git/ARM?path=%2FAPIM%2FSampleAlgorithm%2Fsamplealgorithm.yml
- After reviewing the simple YAML configuration example execute the APIMTemplate command line tool in create mode to generate the necessary ARM templates to deploy the API configuration to an APIM instance. An example of the command with appropriate parameters is shown below

        apimtemplate create --configFile "C:\Data\Source\Repos\ARM\APIM\SampleAlgorithm\samplealgorithm.yml"

- After executing the command review the two .json files that were generated; the ARM template and the ARM template parameters file

## Extracting API configuration from API Management

- Explain to the customer that for developers who are using the Azure portal or an extension for Visual Studio or Visual Studio Code to create and maintain their API configurations against a development APIM instance that the extract feature of the APIMTemplate tool is more suited to their needs.
- Discuss how the tool can extract an existing API configuration from an APIM instance and export the configuration to an ARM template. Review the command line arguments for the extract feature found here https://github.com/Azure/azure-api-management-devops-resource-kit/tree/master/src/APIM_ARMTemplate#extractor-arguments.
- Make sure the customer is aware that prior to running the tool in extract mode you must use the az command line tool to login to Azure and select the subscription where the APIM instance is located.
- Execute the APIMTemplate tool in extract mode to extract the API configuration of your sample API from your development APIM instance. An example of the command with appropriate parameters is shown below

        apimtemplate extract --sourceApimName nimccollftaapim --destinationApimName nimccftaapim-prod --resourceGroup nimccollfta-rg --fileFolder "C:\Data\Source\Demo\APIM\ExtractDemo" --apiName author-wcf-service --policyXMLBaseUrl https://nimccollftastg.blob.core.windows.net/apim/policies

- Note how the --apiName parameter can be omitted and the tool will extract the configuration for all APIs defined in the APIM instance.
- Note how the blob storage URL will be used during deployment to find the policy XML files associated with the API configurations. The policy XML files will need to be uploaded to the blob storage account and we will do this in the ADO Build Pipeline.
- When the extract is complete review the files and folders that were generated. Again note the ARM template for the API configuration and its associated template parameters file. Also note that several other ARM templates were created for any associated products, loggers, etc. Point out that any extracted policy XML files are located in the policies folder.

## Source control for API configuration

- Review how both modes of the APIMTemplate tool generate ARM templates to deploy API configurations to APIM. These ARM templates become the source code for our API configuration. The generated ARM templates and associated policy XML files should be checked into source control. If the customer is planning on using the create mode make sure they check in the YAML configuration file as well.
- Discuss how from this point forward any changes made to the API configuration must be reflected in the ARM templates by running the APIMTemplate tool and regenerating the ARM templates and policy XML files. Those modifications should then be checked back into source control.
- Taking this approach allows us to create a CI / CD pipeline in Azure DevOps to deploy our API configuration changes to an APIM instance.

## Create a Build Pipeline for your API configuration

- Now that we have ARM templates in source control that can be used to deploy our API configurations to APIM, talk to the customer about how we can leverage Azure DevOps to build and deploy our configurations to an APIM instance.
- Open Azure DevOps and create a new Build Pipeline using the Empty Template. The input source control repository should contain the ARM templates and XML policy files for the API configurations.
- The Build Pipeline will consist of three tasks and can be built using the classic pipeline editor or YAML.

    1. An Azure File Copy task that will copy the XML policy files from the source control repository to the Azure blob storage account we specified when executing the APIMTemplate tool. This is necessary because the ARM Template deployment step in the release pipeline will not be able to read the policy XML files from the artifact drop location.
    1. An Azure Resource Group Deployment task that will validate the ARM templates for syntax errors. The deployment mode of this task should be set to Validation only.
    1. A Publish Build Artifacts task that will copy the ARM templates to the drop location.

    An example Build Pipeline that you can import into your Azure DevOps subscription can be found here https://nimccoll.visualstudio.com/GitDemo/_git/ARM?path=%2FAPIM%2FPipelines%2FAPIM-ARM-Extract-Build.json

- Execute the Build Pipeline and verify that all steps execute successfully and any polices have been copied to blob storage and the template files are in the drop location of the build artifacts.

## Deploy your API configuration using a Release Pipeline

- Once the build executes successfully create a new Release Pipeline in Azure DevOps using the Empty Job option
- Select the output from the build pipeline created earlier as the artifact input for the release pipeline
- The Release Pipeline will consist of one step

    1. An Azure Resource Group Deployment task that will deploy the API configurations via the ARM templates to the specifed APIM instance. The deployment mode of this task should be set to Incremental to allow API configurations to be updated as developers are making changes without overwriting the entire resource group.

    An example Release Pipeline that you can import into your Azure DevOps subscription can be found here https://nimccoll.visualstudio.com/GitDemo/_git/ARM?path=%2FAPIM%2FPipelines%2FAPIM-ARM-Extract-Release.json

- Create a release for the Release Pipeline and trigger the deployment. Upon successful completion of the release you should be able to view the API configuration in the destination APIM instance.