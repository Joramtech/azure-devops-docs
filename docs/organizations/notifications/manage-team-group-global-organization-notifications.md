---
title: Manage notifications for a team, project, organization, or collection
titleSuffix: Azure DevOps
description: Learn how to configure team, project, and organization/collection notifications for when changes occur to source code, git, work items, and builds in Azure DevOps.
ms.subservice: azure-devops-notifications
ms.assetid: 6edc44d0-2729-46f5-8108-c8a5160a6a7a
ms.custom: cross-project
ms.author: chcomley
author: chcomley
ms.topic: how-to
ms.date: 09/05/2024
monikerRange: '<= azure-devops'
---

# Manage notifications for a team, project, or organization

[!INCLUDE [version-lt-eq-azure-devops](../../includes/version-lt-eq-azure-devops.md)]

You can manage email notifications at team, project, and organization levels when changes occur to work items, code reviews, pull requests, source control files, and builds.

For example, when a high priority work item is assigned to your team's area path, a notification email gets sent to the team. For more information, see [Notification types](about-notifications.md#notification-types).

## Prerequisites

[!INCLUDE [prerequisites-notifications](includes/prerequisites-notifications.md)]

[!INCLUDE [note-smtp-server](includes/note-smtp-server.md)]

## Create an email subscription

A subscription lets you control what your team is notified of and how the team receives those notifications. For more information, see [notification types](about-notifications.md#notification-types).

::: moniker range="azure-devops"

1. Sign in to your organization (```https://dev.azure.com/{yourorganization}```).
2. Select **Project settings** > **Notifications**.

    :::image type="content" source="media/nav-team-notifications-hub-newnav.png" alt-text="Screenshot of Project settings and Notifications highlighted":::

3. Select **New subscription**. 

    ![Screenshot of New subscription highlighted.](media/new-subscription-newnav.png) 

4. Select the type of activity you want your team to be notified of.

	![Screenshot of select event category and template page.](media/new-sub-page-preview.png)

1. Provide a description to help you identify the subscription later.

    ![Screenshot of a description provided.](media/new-sub-description.png)

1. Choose which team members should receive a notification:

    ![Screenshot of Deliver to and Roles dropdown menus.](media/new-sub-team-delivery-by-role.png)

   Choose from one of the following delivery options:

     | **Delivery option**    | **Description**   | 
     | --------------------|-------------------|  
     | **Team members by role** | Only certain team members associated with the event are notified. For example, for work item changes, you might only want the current assignee of the work item to receive a notification. |  
     | **Team preference**      | Use the team's default delivery preference. For more information, see [Manage delivery settings](#manage).   |  
     | **Custom email address** | Send an email to a specified email address.    |  
     | **All team members**     | Send an individual email to each member of the team.        | 
     | **SOAP**  | Send email notifications to subscribers of SOAP service.     |

   For certain activities, when you select **Team members by role**, you can choose to have the user that initiated the activity receive a notification. This notification is controlled by the **Skip initiator** checkbox. By default, this box is checked, meaning the user that starts the change isn't notified about it.

   > [!TIP]
   > For **Team members by role**, each role is fairly self-explanatory. However, the following two roles may need some further explanation. 
   > 
   > **Changed reviewers** applies to any reviewer added or deleted, as a result of policies defined for the set of files. For example, a push to a pull request (PR) could introduce a change to File1.cs. If there’s a policy which says that Person A needs to review changes to File1.cs, they’d be in the Changed reviewers role for that iteration of the PR. 
   > 
   > The **Reset reviewers** role is related to the “reset votes” policy. For example, the repo configured the policy, “Reset votes on new pushes”. Person B, who was required on the PR, already approved this PR. Because of the reset votes policy, their vote is reset. Thus, they're in the Reset reviewers role for that iteration.

2. Choose whether you want to receive notifications about activity in all projects or only a specific project.

    ![Screenshot of selected scope.](media/new-sub-scope.png)

3. Optionally, configure more filter criteria. For fields, such as Created By, that require a user as a value, enter the username or email address of the user.

    ![Screenshot of configuring additional filter criteria.](media/new-sub-filter-conditions.png)

4. Select **Finish** to save the new subscription.

::: moniker-end  

::: moniker range="< azure-devops"

1. Sign in to your organization (```https://dev.azure.com/{yourorganization}```).
2. Select **Project settings** > **Notifications**.

    :::image type="content" source="media/nav-team-notifications-hub-newnav.png" alt-text="Screenshot of Project settings and Notifications highlighted":::

3. Select **New subscription**.

    ![New subscription is highlighted.](media/new-subscription-newnav.png) 

4. Select the type of activity you want your team to be notified of.

	![Select event category and template.](media/new-sub-page1.png)

5. Provide a description to help you identify the subscription later.

    ![Provide a description.](media/new-sub-description.png)

6. Choose which team members should receive a notification:

    ![Select role.](media/new-sub-team-delivery-by-role.png)

   Choose from one of the following delivery options:

   | Delivery option          | Description                                                                                                                                                                                |
   |--------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
   | **Team members by role** | Only certain team members associated with the event are notified. For example, for work item changes, you might only want the current assignee of the work item to receive a notification. |
   | **Team preference**      | use the team's default delivery preference. For more information, see [Manage delivery settings](#manage).                                                                                 |
   | **Custom email address** | Send an email to a specified email address.                                                                                                                                                |
   | **All team members**     | Send an individual email to each member of the team.                                                                                                                                       |

    For certain activities, when you select **Team members by role**, you can choose to have the user that initiated the activity receive a notification. This notification is controlled by the **Skip initiator** checkbox. By default, this box is checked, meaning the user that starts the change isn't notified about it.

   > [!TIP]
   > For **Team members by role**, each role is fairly self-explanatory. However, the following two roles may need some further explanation. 
   > **Changed reviewers** applies to any reviewer added or deleted, as a result of policies defined for the set of files. For example, a push to a pull request (PR) could introduce a change to File1.cs. If there’s a policy which says that Person A needs to review changes to File1.cs, they’d be in the Changed reviewers role for that iteration of the PR. 
   > The **Reset reviewers** role is related to the “reset votes” policy. For example, the repo configured the policy, “Reset votes on new pushes”. Person B, who was required on the PR, already approved this PR. Because of the reset votes policy, their vote is reset. Thus, they're in the Reset reviewers role for that iteration.

7. Choose whether you want to receive notifications about activity in all projects or only a specific project.

    ![Select scope](media/new-sub-scope.png)

8. Optionally, configure more filter criteria.

    ![Configure additional filter criteria.](media/new-sub-filter-conditions.png)

9. Select **Finish** to save the new subscription.

::: moniker-end  

<a name="manage"></a>

> [!TIP]
> If you don't want to receive a notification for an event that you initiated, you can turn on the option, *Skip initiator*. For more information, see [Exclude yourself from notification emails for events that you initiate](exclude-self-from-email.md).

## Manage global delivery settings

Global notifications apply to all **projects** defined for an organization or collection. 
Choose to allow or block delivery of emails for all subscriptions owned by a team or a group. It's a default setting, which applies only if the team or group hasn't explicitly set the option. For more information, see [Global notifications](about-notifications.md#global-notifications).

::: moniker range="azure-devops"

> [!TIP]
> We don't support organization-wide notifications. As an alternative, you can provide an email distribution list that goes to your entire organization. Also, you can generate a banner with the [**az devops banner command**](../../organizations/settings/manage-banners.md) that all users see when they sign in.

::: moniker-end

::: moniker range="azure-devops-2020"

> [!TIP]
> You can send an email to all collections in an application tier. See [Configure an SMTP server and customize email for alerts and feedback requests](/azure/devops/server/admin/setup-customize-alerts). Also, you can generate a banner to communication with users without sending out mass emails. For more information, see [Add and manage information banners in Azure DevOps](../../organizations/settings/manage-banners.md).

::: moniker-end

::: moniker range=" azure-devops-2019"
> [!TIP]
> You can send an email to all collections in an application tier. See [Configure an SMTP server and customize email for alerts and feedback requests](/azure/devops/server/admin/setup-customize-alerts). 

::: moniker-end

[!INCLUDE [opt-out-notification](includes/opt-out-notification.md)]

## Related articles

- [Manage your personal notification settings](manage-your-personal-notifications.md)
- [Set your notification preferences](../../organizations/settings/set-your-preferences.md)
- [Review default and supported notifications](oob-built-in-notifications.md)
- [Follow a specific work item](../../boards/work-items/follow-work-items.md)
- [Change your preferred email address](change-email-address.md)
