# Force Sync Entra SG Users into DV Environment

Many customers utilize Microsoft Entra Security Groups (SG) linked teams in Dataverse to easily onboard and manage user access and security role assignment (described in the sections below). When utilizing a primarily automated onboarding process like this, admins often need to do more setup on users once they are synced into the environment, such as move to a user to a specific business unit, or set Manager or other attributes on the User record. 

The challenge is that the syncing of users into Dataverse can take a long time to happen, and may not occur reliably until the first time a user attempts to access a resource in that environment. This results in no user record being found in the environment to do required configurations on.

Admins can use this sample tool to force the sync of users that have been added to the environment but are not yet visible as members of the environment for additional configuration. 

| Type | Name | Solution Name |
|------|------|---------------|
| Power Automate Cloud Flow | Force Sync Entra SG Users into DV Environment | UserGovernance_x_x_x_x.zip|

Find these sample tools in solutions in the the [Samples](./Samples/) folder of this repo. 

## Instructions for use
1. Deploy the flow into any environment in the tenant, as an Admin
2. Collect the following:
    * Object ID of the Entra Security Group
    * Environment ID of the target Dataverse Environment
3. Run the flow, (Admin rights on the target environment) entering the above data
4. Confirm users are now visible in the environment

## Background Concepts
 * Sharing an app with an Entra SG in the Maker Space automatically creates a SG Linked Team in that Dataverse environment (these teams can also be manually created in Dataverse)
 * Sharing Security Roles with an Entra SG when sharing an app assigns those roles to the created SG Linked Team
 * Adding/removing members of the Entra SGs in Azure syncs membership to the linked Dataverse teams
 * Business Unit and Security Role assignment of SG linked teams can be managed from the environment Admin Settings once created
 * Members of these Entra SGs must also be members of the top level Security Group tied to the Dataverse environment (if one is assigned) in order to be enabled in the envrionment

 References:
 * [Control user access to environments: security groups and licenses - Power Platform | Microsoft Learn](https://learn.microsoft.com/en-us/power-platform/admin/control-user-access)
 * [Add users to an environment automatically or manually - Power Platform | Microsoft Learn](https://learn.microsoft.com/en-us/power-platform/admin/add-users-to-environment)
 * [Control user access to environments: security groups and licenses - Power Platform | Microsoft Learn](https://learn.microsoft.com/en-us/power-platform/admin/control-user-access)
  * [Manage group teams - Power Platform | Microsoft Learn](https://learn.microsoft.com/en-us/power-platform/admin/manage-group-teams#create-a-group-team)

## Example Deployment/Onboarding Steps using this capability:

1. Create/Manage Environment-level Entra Security Group, assign to Environment
    * For "Front Door Access", but does not give access to apps or data
2. Create/Manage Entra Security Groups for each user role/app combination
    * e.g. "My Enterprise App" requries two custom security roles, therefore we create two SGs: 
        * "My Enterprise App - Basic Users"
        * "My Enterprise App - Power Users"
3. Create SG-linked teams in Dataverse, either manually from the PP Admin Center or by sharing the app and security roles with the Entra SGs from the Maker Space
4. Run the `Force Sync Entra SG Users into DV Environment` cloud flow, once for each of the SGs in step 2
5. Complete any additional user configuration required
4. In maintenance mode:
    * Adding new members to SGs automatically syncs them into the environment and teams, where they inherit the appropriate security roles
    * Removing members from SGs revokes their security roles and/or access to the environment
    * The `Force Sync Entra SG Users into DV Environment` can be run for a SG anytime membership is changed and newly added members are not found when additional user configuration is needed




