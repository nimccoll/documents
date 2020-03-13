# Azure DevOps Security - slideless delivery

## Service Description

The default security permissions assigned by Azure DevOps when a Team project is created provides broad access to the default project security groups such as Readers and Contributors. This access extends to all of the key subsystems of Azure DevOps including Boards, Repos, and Pipelines. In many cases as customers begin to create separate Teams to represent the actual projects that will be contained within the Team project they want the ability to restrict access to the Boards, Repos, and Pipelines of a given Team to its team members and not to the default security groups associated with the entire Team project.

## Agenda

- Azure DevOps Security Overview
- Setting permissions for Azure Boards
- Setting permissions for Azure Repos
- Setting permissions for Azure Pipelines
- Setting permissions for Azure Test Plans
- Setting permissions for Azure Artifacts

## Prerequisites

- The ability to join a Microsoft Teams call and screen share.
- Access to an Azure DevOps subscription with the ability to create a Team Project, create a Team within that Team Project, and manage security permissions for Boards, Repos, Pipelines, Test Plans, and Artifacts.

    This will be a demonstration-led session. The customer should have access to an Azure DevOps subscription so they can follow along and configure their own environment during the delivery.  

## Azure DevOps Security Overview

- Explain the concept of access levels and security groups within Azure DevOps (https://docs.microsoft.com/en-us/azure/devops/organizations/security/about-permissions?view=azure-devops&tabs=preview-page%2Ccurrent-page#permissions-versus-access-levels)
- Talk through our guidance regarding assignment of access levels and security groups (https://docs.microsoft.com/en-us/azure/devops/organizations/security/permissions-access?view=azure-devops)
- Review the default permissions for each of the following ADO subsystems
    - Dashboards (https://docs.microsoft.com/en-us/azure/devops/organizations/security/permissions-access?toc=%2Fazure%2Fdevops%2Fsecurity-access-billing%2Ftoc.json&bc=%2Fazure%2Fdevops%2Fsecurity-access-billing%2Fbreadcrumb%2Ftoc.json&view=azure-devops#dashboards-charts-reports-and-widgets)
    - Power BI Integration and Analytics View (https://docs.microsoft.com/en-us/azure/devops/organizations/security/permissions-access?toc=%2Fazure%2Fdevops%2Fsecurity-access-billing%2Ftoc.json&bc=%2Fazure%2Fdevops%2Fsecurity-access-billing%2Fbreadcrumb%2Ftoc.json&view=azure-devops#power-bi-integration-and-analytics-views)
    - Boards (https://docs.microsoft.com/en-us/azure/devops/organizations/security/permissions-access?toc=%2Fazure%2Fdevops%2Fsecurity-access-billing%2Ftoc.json&bc=%2Fazure%2Fdevops%2Fsecurity-access-billing%2Fbreadcrumb%2Ftoc.json&view=azure-devops#azure-boards)
    - Repos (https://docs.microsoft.com/en-us/azure/devops/organizations/security/permissions-access?toc=%2Fazure%2Fdevops%2Fsecurity-access-billing%2Ftoc.json&bc=%2Fazure%2Fdevops%2Fsecurity-access-billing%2Fbreadcrumb%2Ftoc.json&view=azure-devops#azure-repos>
    - Pipelines (https://docs.microsoft.com/en-us/azure/devops/organizations/security/permissions-access?toc=%2Fazure%2Fdevops%2Fsecurity-access-billing%2Ftoc.json&bc=%2Fazure%2Fdevops%2Fsecurity-access-billing%2Fbreadcrumb%2Ftoc.json&view=azure-devops#azure-pipelines)
    - Test Plans (https://docs.microsoft.com/en-us/azure/devops/organizations/security/permissions-access?toc=%2Fazure%2Fdevops%2Fsecurity-access-billing%2Ftoc.json&bc=%2Fazure%2Fdevops%2Fsecurity-access-billing%2Fbreadcrumb%2Ftoc.json&view=azure-devops#azure-test-plans)
    - Artifacts (https://docs.microsoft.com/en-us/azure/devops/organizations/security/permissions-access?toc=%2Fazure%2Fdevops%2Fsecurity-access-billing%2Ftoc.json&bc=%2Fazure%2Fdevops%2Fsecurity-access-billing%2Fbreadcrumb%2Ftoc.json&view=azure-devops#azure-artifacts)
- Review the concept of permission inheritance in Azure DevOps (https://docs.microsoft.com/en-us/azure/devops/organizations/security/about-permissions?view=azure-devops&tabs=preview-page%2Ccurrent-page#inheritance-and-security-groups)

## Setting Permissions for Azure Boards

Reference: https://docs.microsoft.com/en-us/azure/devops/organizations/security/set-permissions-access-work-tracking?view=azure-devops

Setting permissions for Azure Boards involves setting permissions on an Area Path within the work tracking system. Typically each Team within a Team project has an Area Path that is associated with the Team. By setting permissions on the Area Path we can control who has access to the work items and queries that belong to the Team.

- Create a new Team within the Team Project. Make sure to take the default option to create an Area Path associated with the Team.
- Navigate to the Project Settings for the current Team Project.
- Select Project Configuration under Boards
- Click Areas to display the Area Paths associated with the Team Project.
- Click the ellipsis next to the Area Path that corresponds to the Team you created earlier and choose Security from the context menu.
- When the permissions dialog for the Team is displayed discuss how the default inheritance gives the default Team Project Contributors and Team Project Readers default access to the new Team's work items.
- Discuss how we can remove access to the default Contributors group and limit write access to work items in this Area Path to only members of the newly created Team.
    - Disable Inheritance
    - Add the security group for the Team and give them the same access as the default Contributors group
    - Remove the default Contributors group
- If necessary, you can also remove the default Readers group to limit read-only access to the work items that belong to the Team.

## Setting Permissions for Azure Repos

Reference: https://docs.microsoft.com/en-us/azure/devops/organizations/security/set-git-tfvc-repository-permissions?view=azure-devops

The process for setting permissions on an Azure Repo is very similar to setting permissions on an Area Path.

- Create a new repository within the Team Project. We will limit access to this repository to the Team we created earlier.
- Navigate to the Project Settings for the current Team Project.
- Select Repositories under Repos
- Click the name of the Repository we created earlier from the Repositories list in the middle of the page.
- Click Security to display the permissions currently assigned to the repository.
- Discuss how the default inheritance gives the default Team Project Contributors and Team Project Readers default access to the newly created repository.
- Discuss how we can remove access to the default Contributors group and limit write access to the repository to only members of the newly created Team.
    - Disable Inheritance
    - Add the security group for the Team and give them the same access as the default Contributors group
    - Remove the default Contributors group
- If necessary, you can also remove the default Readers group to limit read-only access to the repository.

## Setting Permissions for Azure Pipelines

Reference: https://docs.microsoft.com/en-us/azure/devops/pipelines/policies/set-permissions?view=azure-devops

Permissions for build and release pipelines can be set at the folder level as well as on an individual build or release pipeline. The challenge here is that the user interface behavior varies between the Build and Release pipeline functionality. We will cover each separately.

### Setting Build Pipeline permissions (Folders)

- Click Pipelines in the left hand navigation and then click Builds.
- Switch to the Folder view by clicking the Folders icon.
- Create a new folder and give it the same name as the Team we created earlier. We will use this folder to store all Build pipelines for the Team.
- Click the name of the folder you just created in the folders list.
- Click the ellipsis in the upper right hand corner above the Filter icon and choose Security from the context menu.
- When the permissions dialog for the folder is displayed discuss how the default inheritance gives the default Team Project Contributors and Team Project Readers default access to the build pipelines within the folder.
- Discuss how we can remove access to the default Contributors group and limit the ability to create builds within the folder to only members of the newly created Team.
    - Disable Inheritance
    - Add the security group for the Team and give them the same access as the default Contributors group
    - Remove the default Contributors group
- If necessary, you can also remove the default Readers group to limit read-only access to the build pipelines within the folder.

### Setting Build Pipeline permissions (Individual Pipelines)

Indicate to the customer that setting permissions at the folder level for a Team is the most efficient way to manage permissions for all of the build pipelines that belong to a Team. However, if necessary, you can set permissions on an individual build pipeline by following the procedure below.

- Create a new build pipeline within the folder you created earlier. You can simply use the empty template for the pipeline details.
- Once you have created and saved the pipeline navigate back to the Folder view.
- Click the newly created build pipeline.
- Click the ellipsis in the upper right hand corner above the Filter icon next to the Edit and Queue buttons and choose Security from the context menu.
- When the permissions dialog for the build pipeline is displayed discuss how the default inheritance gives the default Team Project Contributors and Team Project Readers default access to the build pipeline.
- Discuss how we can remove access to the default Contributors group and limit the ability to edit the build pipeline to only members of the newly created Team.
    - Disable Inheritance
    - Add the security group for the Team and give them the same access as the default Contributors group
    - Remove the default Contributors group
- If necessary, you can also remove the default Readers group to limit read-only access to the build pipeline.

### Setting Release Pipeline permissions (Folders)

- Click Pipelines in the left hand navigation and then click Releases.
- Switch to the Folder view by clicking the Folders icon.
- Create a new folder and give it the same name as the Team we created earlier. We will use this folder to store all Release pipelines for the Team.
- Click the name of the folder you just created in the folders list.
- Click the ellipsis next to the New drop-down menu and choose Security from the context menu.
- When the permissions dialog for the folder is displayed discuss how the default inheritance gives the default Team Project Contributors and Team Project Readers default access to the release pipelines within the folder.
- Discuss how we can remove access to the default Contributors group and limit the ability to create release pipelines within the folder to only members of the newly created Team.
    - Disable Inheritance
    - Add the security group for the Team and give them the same access as the default Contributors group
    - Remove the default Contributors group
- If necessary, you can also remove the default Readers group to limit read-only access to the release pipelines within the folder.

### Setting Release Pipeline permissions (Individual Pipelines)

Indicate to the customer that setting permissions at the folder level for a Team is the most efficient way to manage permissions for all of the release pipelines that belong to a Team. However, if necessary, you can set permissions on an individual release pipeline by following the procedure below.

- Create a new release pipeline within the folder you created earlier. You can simply use the empty job for the pipeline details.
- Once you have created and saved the pipeline navigate back to the Folder view.
- Click the newly created release pipeline.
- Click the ellipsis in the upper right hand corner above the Filter icon next to the Edit and Create Release buttons and choose Security from the context menu.
- When the permissions dialog for the release pipeline is displayed discuss how the default inheritance gives the default Team Project Contributors and Team Project Readers default access to the release pipeline.
- Discuss how we can remove access to the default Contributors group and limit the ability to edit the release pipeline to only members of the newly created Team.
    - Disable Inheritance
    - Add the security group for the Team and give them the same access as the default Contributors group
    - Remove the default Contributors group
- If necessary, you can also remove the default Readers group to limit read-only access to the release pipeline.

## Setting Permissions for Azure Test Plans

Reference: https://docs.microsoft.com/en-us/azure/devops/organizations/security/set-permissions-access-test?view=azure-devops&tabs=preview-page

Permissions for managing test plans and test suites are assigned at the Area Path associated with the Team. Refer to the steps discussed earlier under Setting Permissions for Azure Boards. Specifically make sure to call out the Manage test plans and Manage test suites permissions on the Access Control Summary on the Permissions dialog box.

## Setting Permissions for Azure Artifacts

Reference: https://docs.microsoft.com/en-us/azure/devops/artifacts/feeds/feed-permissions?view=azure-devops

The user interface behavior for setting permissions on Azure Artifacts varies greatly from the other subsystems that make up Azure DevOps.

- Click Artifacts in the left hand navigation.
- If you do not have any feeds, create a new feed.
- Select a feed from the drop-down list box.
- Click the gear icon in the upper right to open the feed settings page.
- Click the Permissions link at the top of the Feed Settings page.
- Add a user or group by clicking the +Add users/groups link at the top of the page.
- Select a user or group to add on the Add users or groups dialog box and choose their role:
    - Owner
    - Contributor
    - Collaborator
    - Reader
- Click Save
- To remove a user's or group's access click the user or group name in the User/Group list to select them and then click the Delete link at the top of the page.