# Okta Identity Governance - Access Requests (prepwork)


## Assigning the Access Request application to all users
We will now explore our Okta Access Request capabilities.

Navigate to your Okta **admin dashboard → Applications → Applications** and look for the app named Request Access. 

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step2-1.png)

Click on the app and go to the **assignments** tab. Click on the blue **Assign** button, select *Assign to Groups*. Find and assign the app to the *Everyone* group. This is a built in group that contains all users in your Okta tenant. 

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step2-1a.png)

Now all users should be able to access the *Request Access* portal from which they can request access to resources (once we’ve created some access request flows).

## The Access Requests admin dashboard

Now it’s finally time to go into the Request admin dashboard. From the Okta admin dashboard, navigate to **Identity Governance → Access Requests**. This should open up a new tab which presents you with the access requests admin dashboard. 

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step2-6.png)

### Create an Access Requests Team

Teams are an Access Request grouping mechanism. Workflows must be assigned to a Team, and Teams can be used to scope who can run a workflow or participate in it. There is a default team created called *IT*. For the sake of the exercise, we will **create** a team called **Request Admins**.

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step2-7.png)

In the **Access Requests UI**, navigate to Teams, click on the **Add Team** button (top right). Fill in the Name, make sure you are in the Members field and click on Create team. We will use this team in an access request flow later on.

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step2-8.png)

### Complete Okta Integration

The integration between Access Requests and Okta was partially configured when Okta Identity Governance was enabled for the Okta org. Whenever a new Team is created, the Okta configuration must be updated to allow workflows assigned to the team to be able to perform the appropriate operations in Okta.

- In the **Access Requests UI**, go to the **Settings** menu item
- Click on the **Edit connection** button in the Okta tile
- Click the **Select teams** dropdown and select all of the teams (IT + newly created team). 
- All of the Actions should be enabled.
- Click on **Update connection**.

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step2-2.png)

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step2-3.png)

### Enable the Access Request teams to use Okta Groups and Applications as resources

From the **Access Requests UI** navigate to **Settings -> Resources**. The first resource-group called *Applications*, should be auto-selected. **Click** on **Manage Access**.

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step2-4.png)

Make sure all of the teams are toggled on for all Applications. **Do the same for Groups**. Your list of applications and groups might look a bit different from the screenshot, but that is ok. You may also not yet see Okta Workflows in the Resources list. But we will adress that later in the lab.

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step2-5.png)