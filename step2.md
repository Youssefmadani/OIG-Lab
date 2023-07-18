# OIG Access Requests

The first capability we will explore is Access Requests. It's the most complex of the three capabilities to configure, but once configured it can be used to grant access that can be subsequently reviewed/revoked in Access Certifications.

## Assigning the Access Request application to all users

Navigate to your Okta **admin dashboard → Applications → Applications** and look for the app named Request Access. Click on the app and go to the **assignments** tab. Click on the blue **Assign** button, select *Assign to Groups* , and find and assign the app to the *Everyone* group. This is a built in group that contains all users in your Okta tenant. 

![](/Images/step2-1.png)

Now all users should be able to access the *Request Access* portal from which they can request access to resources (once we’ve created some access request flows).

## The Access Requests admin dashboard

Now it’s finally time to go into the Request admin dashboard. From the Okta admin dashboard, navigate to **Identity Governance → Access Requests**. This should open up a new tab which presents you with the access requests admin dashboard. 

### Create an Access Requests Team

Teams are an Access Request grouping mechanism. Workflows must be assigned to a Team, and Teams can be used to scope who can run a workflow or participate in it. There is a default team created called IT. For the sake of the exercise, we will create a team called
Request Admins.


In the Access Requests UI, navigate to Teams, click on the **Add Team** button (top right). Fill in the Name, make sure you are in the Members field and click on Create team. We will use this team in an access request flow later on.

### Complete Okta Integration

The integration between Access Requests and Okta was partially configured when Okta Identity Governance was enabled for the Okta org. Whenever a new Team is created, the Okta configuration must be updated to allow workflows assigned to the team to be able to perform the appropriate
operations in Okta.

- In the Access Requests UI, go to the Settings menu item
- Click on the Edit connection button in the Okta tile
- Click the **Select teams** dropdown and select your new team. 
- All of the Actions should be enabled.
- Click on **Update connection**.

![](/Images/step2-2.png)

![](/Images/step2-3.png)

### Create App and Group resource lists to be used in Access request flows

From the Access Requests UI navigate to **Settings -> Configuration Lists** and click on **Create new list**. Create two lists called **Applications** and **Groups**. In the same Configuration Lists tab click on the three dots next to Applications. Click on Edit list.

![](/Images/step2-4.png)


Make sure the Request Admins team is selected and click on Add item at the bottom left of the pop up. Look up the following applications that have been automatically added to your Okta tenant as part of this lab and add them to the two resource lists we have just created.

- Application 1
- Application 2
- Application 3 

![](/Images/step2-4.png)