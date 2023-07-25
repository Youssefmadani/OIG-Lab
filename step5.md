# Access Certifications

The next capability we will explore is Access Certification. Access Certification (aka attestation, recertification etc.) is how we validate that a user still needs the access to specific resources/permission-sets that they have. Certification campaigns may be run periodically, or there may be continuous certification when user roles change. You can also use our API and/or Workflows to dynamically certify access. You might for example create a flow which checks for unused applications and kick off an access review based on lack of usage.

## Creating an Access Certification Campaign

Access certification is built into the Workforce Identity Cloud. There is an administrative interface to create and manage campaigns, and an end-user interface for participating in campaigns.

We will create a campaign for resources in your environment. The example below will review application assignment, where the application is the one that was used in the Access Request section earlier. We can use any application that has been assigned to users. The users should have a manager assigned to them and the manager will be configured to be the reviewer during the lab.

## Create a New Campaign

To create a new campaign:
- Login into Okta as an administrator and go to the Admin console
- Find the new Identity Governance menu item and expand it
- Click on Access Certifications

![](https://github.com/Youssefmadani/OIG-Lab/tree/main/Images/
step5-1.png)

You should now see the *Access Certification Campaigns* screen. This page contains all of your Active, Scheduled and Closed campaigns. All of them should be empty at this point in time. Click on **Create Campaign**.

![](https://github.com/Youssefmadani/OIG-Lab/tree/main/Images/
step5-2.png)

The first thing we will have to decide is which type of campaign we'd like to run. We can choose between resource campaigns and user campaigns. 

## Resource campaign
Resource campaigns focus on setting the resource scope for your compaign so that you can review all users who have access to those resources. This campaign type helps you review access to sensitive resources and helps you meet compliance requirements.

## User campaign

User campaigns focus on defining the user scope for your campaign so that you can do a comprehensive review of all resources assigned to those users. This campaign type helps you review users’ access to resources when specific events happen, such as department, role, or project change.

For now, choose **Resource Campaign**. The wizard will walk us through five pages of settings to configure the campaign.

![](https://github.com/Youssefmadani/OIG-Lab/tree/main/Images/
step5-4.png)

## General Settings

The first being the General page. On the General page of the wizard, give the campaign a name, a description (optional) and specify a start date (notice the date/duration options). Click on **Next** at the bottom right.

![](https://github.com/Youssefmadani/OIG-Lab/tree/main/Images/
step5-5.png)

## Resources Settings

The Resources page is where you decide what you are certifying – group membership or application assignment. (Remember that groups can be configured to represent roles). You can only select one Applications or Groups as the resource type, but you can select multiple apps or groups.

For the  **Type**, go ahead and choose **Applications**

![](https://github.com/Youssefmadani/OIG-Lab/tree/main/Images/
step5-6.png)

Under **Select Applications** search for the application called ApplicationPH. This is a dummy application that has been automatically added to your Okta tenant as part of the lab. Click on **Next** at the bottom right.

### Users Settings

On the Users page, you specify the scope of the users in the campaign. Are all users assigned to an application to be reviewed, or only some? Do you need to exclude specific users for some reason?

- Leave the **All users assigned to the resource** option selected

```
Note: 
If you chose to Specify user scope, you can use the Okta Expression Language to filter in users based on attributes stored in Universal Directory. We won’t cover this in this guide.
```

Click on **Next** at the bottom right. 

![](https://github.com/Youssefmadani/OIG-Lab/tree/main/Images/
step5-7.png)

## Reviewer Settings

The reviewers are the Okta users who will review the access. There could be a single (static) reviewer specified as an Okta user. Or you can specify a dynamic reviewer, where the reviewer is based on some data in the Universal Directory (we will do this).

Go ahead and choose Manager as the First-level Reviewer.

![](https://github.com/Youssefmadani/OIG-Lab/tree/main/Images/
step5-8.png)

This will expand the first-level reviewer option and allow us to specify a Fallback reviewer in cases where the user's manager isn't defined in Okta. Go ahead and select yourself, the super-admin, as a Fallback reviewer.

![](https://github.com/Youssefmadani/OIG-Lab/tree/main/Images/
step5-9.png)

Remember that the dummy users in your Okta org were pre-populated during org creation as well as some attributes in their user profile. So the users should have a manager assigned to them. To check this, we can click on **preview reviewer** and look up a user to verify that we see the expected behavior. 

![](https://github.com/Youssefmadani/OIG-Lab/tree/main/Images/
step5-10.png)

As you can see in the screenshot above, Youssef is the manager and therefor the reviewer for John Wick.

Okta Identity Governance currently supports **two levels** of reviewers. (out of scope for this workshop) You might want to recertify access based on multiple reviews. Maybe the second level reviewer needs to be the group-owner. Or maybe we require more flexibility? Just like how we can use Okta expression language to specify user scope, we can also use Okta expression language to dynamically decide who the first/second-level approver will be. However, this too is out of scope of this workshop.

![](https://github.com/Youssefmadani/OIG-Lab/tree/main/Images/
step5-11.png)

For more information on how Okta Expression language can be leveraged to dynamically define reviewers, check out [this](https://help.okta.com/oie/en-us/Content/Topics/identity-governance/access-certification/iga-el-examples.htm?cshid=csh-el-eg-reviewers#reviewers) page. 

Let's go ahead with only a First-level reviewer. 

Notice the Notification settings in the same page. We will not enable any of them, but you have the option to send emails to reviewers at campaign launch, x days before campaign end, and on campaign end. Feel free to enable them and monitor the email inboxes, but we won’t include it in the steps below.

Go ahead and click **Next** if your Reviewer settings look like the screenshot below.

![](https://github.com/Youssefmadani/OIG-Lab/tree/main/Images/
step5-12.png)

## Remediation

Now on to arguably the most important step in this flow. What happens when the final reviewer makes a decision? We have a few options here. 

### Reviewer approves Access:

*Nothing happens and the user will continue to enjoy access to a specific resource/role*.

### Reviewer decides to revoke access:

- We can choose to only flag the resource to be removed but not actually remove access right away.
- Or we can choose to automatically remove access as soon as the reviewer decides access should be revoked. (There may be implications on this depending on how the access was granted).


**Note**: *If you choose **Remove access from user**, but Okta can’t do it due to an internal policy, it will be flagged for manual remediation in the campaign results.*

You also need to decide what happens if there are unreviewed items at the end of the campaign – should they be removed or left as is?

For **Reviewer revokes access:** choose **Remove access from user** and for **Reviewer does not respond:** choose **Don't take any action**. The result should look like the screenshot below. If you want to make any changes to any of the settings you can click on the sections on the left hand side of the page and make the appropriate changes. 

- Go ahead and click on **Schedule Campaign** 

![](https://github.com/Youssefmadani/OIG-Lab/tree/main/Images/
step5-13.png)

The campaign will now appear under the Scheduled tab.

![](https://github.com/Youssefmadani/OIG-Lab/tree/main/Images/
step5-14.png)

Clicking on the campaign reveals the Campaign settings:

![](https://github.com/Youssefmadani/OIG-Lab/tree/main/Images/
step5-15.png)

The campaign is scheduled to start at some point in the future. Let's go ahead and launch this campaign ahead of the scheduled time.
- Click on the **Actions** dropdown at the top right and select **Launch**. (This is also where you could edit/delete the campaign).
- In the launch campaign confirmation window select **Launch** once more.

**(TBC, encountered bug which fails launch of campaign. Should be fixed this week by the team. - p77 of the guide)**
