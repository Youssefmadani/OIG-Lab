# Creating a request flow with logic, an approver and a workflows integration

### What will we do in this section:
Now we will create a request flow that does require approval from the manager. We could have multiple steps of approval, for example, only assign the resource if both the manager and the group owner approve the request. But in this case manager approval will suffice. We will also incorporate some logic into the flow which will enable us to only perform certain tasks if others have been completed. And then finally, upon manager approval, we will trigger a specific workflow which will enable us to grant the requestor elevated permissions for the requested time-frame. 

**IMPORTANT: This specific flow can be downloaded from the top of this page and imported into your Okta Workflows console!** We will provide instructions later in this guide on how to import the flowpack into your environment.

<!---During this lab we will allow end-users to request application-admin privileges for a maximum duration of 10 days. If the user requests such access of 5 days, they will be granted those permission for 5 days. If they request such access for more than 10 days, they will only get such access for 10 days. -->

### Setup Okta Workflows and integrate specific flows with the Access Request console

#### Import a flowpack that will be called at the end of an access request flow.
We've seen that it is possible to end an access request flow with a simple group- or app-assignment. But what if you want to do something more complex? Or what if you want to perform a sequence of actions in different applications? That's where Okta Workflows comes in. 

#### High level overview of the flow
So, once the access request has been approved, we will trigger the flow. The flow will:
- Capture information from the access request flow (such as end-user-requested end-time and business justification).
- Put the user in an Okta-group with application-admin permissions (which he/she will inherit).
- Once the requested end-date OR 10 days have passed - whichever is shorter - the user will be auto-removed from the group and lose all of his/her app-admin permissions.

#### Adding a flowpack (template) to your Workflows Console

The flowpack that we will be adding to our Workflows environment can be found here:
https://cdn.demo.okta.com/labs/oig-for-partners/accessRequestApplicationAdminRole.flow
Go ahead and download the above flow-pack, we will import this flowpack in a few minutes.

Navigate to your **Okta Admin dashboard --> Workflow --> Workflows console**
The Workflows console opens in a new tab. 

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step4-c.png)

Once in the Workflows console, navigate to **Flows** (from the top nav-bar) and next to **Folders** (on your left-hand side) click on the **+** Icon.

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step4-d.png)

Give the folder a name and click on **Save**.

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step4-e.png)

Now on the top-right click on **Actions** and then on **Import**. Now choose the flowpack that we asked you to download. 

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step4-f.png)

For your convenience, here is the link again:
https://cdn.demo.okta.com/labs/oig-for-partners/accessRequestApplicationAdminRole.flow

Drag and drop the file in the import field, or click on Choose file from computer. 

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step4-g.png)

Once succesfuly imported, your display should look like this:

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step4-h.png)

Go ahead and toggle the flow in the **ON** state. You should get an error that prompts you to **First connect your apps**. 

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step4-i.png)

This will also immediately open op the flow and reveal the cards in the flow that require a connection to an app. The only apps that we use in this flow is the Okta application itself, so we only need to establish a connection between Okta Workflows and Okta itself.

Under the Okta **Read User Card**, click on **Choose Connection** and then **+ New Connection**. 

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step4-j.png)

We need to fill in 3 parameters to establish a connection with our Okta tenant.

- Domain (without *https://* or *-admin*)
- the Client ID
- the Client Secret

All of these parameters can be obtained from the Okta admin dashboard (that should still be open in another tab. Remember that the Workflows console was also accessed from the Okta Admin dashboard and was opened in a separate tab). Navigate back to the **Okta Admin dashboard**. Go to **Applications --> Applications** and type in **Workflows**. Now select **Okta Workflows OAuth**. 

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step4-k.png)

Now naviagte to the Sign On tab. This is where we can find our Client ID and Client secret. In the URL we can also find our domain URL. We need to copy the domain without the https:// and without the -admin. So in my case that would be:
*igalliance-oktapreview.com* 

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step4-l.png)

Now go ahead and copy and paste the three parameters mentioned earlier into your Workflows console tab. The result should look something like this (with of course your own unique Domain, Client ID etc.):
![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step4-m.png)

Once all of that is done, click on **Create**.

After a few seconds the connection will have been established, and the previous errors should have disappeared. 

### Changing a few parameters

Now we need to change the parameter of two cards in the flow to make it work. We need to update the **groupID** to match the ID of the **application-admins** group. For this, we need to navigate to the **Admin Dashboard --> Directory --> Groups**. Then find and click on the **Application-admins** group.

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step4-n.png)

We need to copy the Group ID of this group, and paste it into the two cards in our workflow. This ensures that the requestor - upon approval - will be put in the right app-admin group. The group ID can be found in the last section of the URL. (See screenshot below). 

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step4-o.png)

Go ahead and copy that, and paste it into the following **two cards**:
- Add User to Group (3rd card)
- Remove User from Group (last card)
 Click on **Save** afterwards. Make sure also to check *Save all data that passes through the Flow?* 

  ![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step4-q.png)

 ![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step4-p.png)

And lastly, ensure the flow is toggled **On**.

 ![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step4-r.png)

Feel free to have a look at the flow by scrolling from the left to the right. In short, we will (upon approval by the manager) add the user to the app-admin group. Wait for a maximum of 10 days or for the amount of days that the user requested access. Whichever is shorter. And then deprovision the user and revoke his/her app-admin privileges.


### Create new admin role and attach resource set

Now we need to create a custom admin role that is allowed to run delegated flows. After that, we assign this role to the Access Request application, allowing the Access Request engine to trigger delegated workflows as part of an Access Request flow.

#### Create a custom admin role with specific permissions

Navigate to your Okta **Admin Dashboard --> Security --> Administrators** and from there go to the **Roles** tab. Now click on **Create new role**.

 ![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step4-s.png)

Now fill in a role name like *Delegated Workflows Admin*. Uncheck everything except Workflow. And the result should look like the image below. This is how we ensure granular permissions for custom admin roles in Okta. Don't forget to click on **Save**!

 ![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step4-t.png)

#### Creating a resource set and attaching that to the previous role

Now go to the *Resources* tab on the same *Administrators* screen and click on **Create new resource set**.

 ![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step4-u.png)

Give the resource set a name and click on **+ Add resource**.

 ![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step4-v.png)

Click on the **input field** --> Select **Workflows** --> Select **All flows** and click on **Save Selection**.

 ![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step4-w.png)

 Click on **CREATE** at the bottom right. And navigate back to the **Roles** tab.

Here you'll want to click on **Edit** next to **Delegated Workflows Admin** (the role we just created) and click on **View/Edit Assignment**.

 ![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step4-x.png)
 

ADD RESOURCES/PERMISSIONS TO THIS ROLE

ASSIGN THE ADMIN ROLE TO ACCESS REQUESTS APP SO THAT IT CAN TRIGGER SPECIFIC WORKFLOWS AS PART OF AN AR FLOW

ENABLE THE WORKFLOWS AS RESOURCES IN OIG


## Creating an Access Request flow with approval and Workflows integration

From the Access Requests Admin UI navigate to Access Requests and click on **Create request type**.


![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step4-1.png)

Type in the Name and give the request type a description. In my case the name is *Application Admin Role*. Configuration should look like the screenshot below.  Click on **Continue**.

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step4-2.png)

We’ll start off the flow with a question to the requester so choose **Question → Add to request type**.

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step4-3.png)

We made the question a required field so the requestor will have to answer before the flow continues. 

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step4-4.png)

For the next question, we will ask the requester until when he requires these elevated permissions. The date that has been input by the requester is the date that will be used by the automated workflow in determining how long the user will be granted elevated permissions.

**Click on *Question* at the bottom of the screen** 

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step4-4a.png)

Now name the question something like what you see on the screenshot below. Make sure to change the Type from **Text** to **Date**. This will enable the requester to select a date. And this date will be fed into the workflow that will be triggered at the end of the request flow. Note also that both questions should be required fields.

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step4-4b.png)

Now let’s add another task which will be an approval task. At the bottom of the screen select **Approval**. 

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step4-5.png)

This task requires the manager of the requester to approve the request.  So let’s fill in the details. Name the task something like “*Manager approval required*”. Make it a required task. And Assigned to needs to be **Requester’s manager**. 

*Notice that we don't have to know **who** the manager of the requester is. This information is contained in the requester's user profile (which has been configured automatically when this lab tenant was spun up by the demo-platform.) In practice, this profile information can be sourced from an HR system, a directory (AD/LDAP) or any other source (that exposes the necessary api's).*

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step4-6.png)

Once the manager has given his/her approval we want to perform an Action as the next step in the flow. Again, at the bottom of the screen, click on Action to add another action/task to the flow. 

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step4-8.png)

This time select **Run a Workflow**.

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step4-9.png)

- Give the action a name such as *Temporarily add user to Application Owner role.* Make the action required if it isn't already. 
- Under *[Okta] Run a workflow* select the workflow that we added in the earlier part of the lab titled *Application Owner Role request flow upon approval*. 
- And finally map all of the request inputs to the inputs that are expected by the delegated workflow as seen in the screenshot below. This enables the workflows engine to use the requestor's input in our automation endeavors.

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step4-10.png)

## Adding logic to the flow

For the last step, we need to add some logic to ensure that the user is only granted elevated permissions upon Approval by the manager. Under **Tasks & Actions** click on the bottom action named **Temporarily add user to Application Owner role.** if not already highlighted. Switch to the logic tab and configure it as shown below: 

```
Only show this task if
Manager approval Required
is approved
```

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step4-11.png)

The end result should look like the image below.

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step4-12.png)

Once finished, click on Publish at the top right to make the resource visible and requestable to the specified audience. 

## Testing if the access request flow works

*For this section we strongly recommend you open two additional Chrome profiles. One which functions as the browser of the requester and another one which functions as the approver (manager). This way you don't have to constantly log in/out. This is especially challenging since we're working with 3 user accounts (requestor, approver and admin).*

Navigate to the Okta login portal and log in with the following credentials.

```
username: bruce.wayne@okta.rocks
password: OktaRocks123!
```

Click on the **Request Access** Application. Bruce should see two requestable resources to him. **Application 1** and the **Application-Admin permission set**. The first flow was created earlier during the lab and is automatically approved upon request. (*If Bruce already has access to a resource it might not be displayed on this screen*). We want to test the second flow so under **Application Owner Role** click on **Request Access**. Fill in the justification field, specify a date until when Bruce requires access, and submit your request. A new task is automatically created that can be followed by Bruce, the approver(s) and admins. We can for example see that manager approval is required. 

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step4-13.png)

### 3 ways of approving access

An approver can approve requests in three ways. None of these ways require that the approver has admin access:

1. Through the request access console.
2. Via e-mail.
3. And through modern chat clients (Slack, Teams)

For the purposes of this lab we will be going with option number one. But to briefly show you how the other options might look like:

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step4-14.png)
`This is how an access request might be approved/denied via e-mail.`

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step4-15.png)
`This is how an access request might be approved/denied through Slack/Teams.`

Now if we log into the access requests dashboard as the manager, we should see that there is a new task pending. 

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step4-16.png)

Clicking on the request reveals details such as why the requester requires access to this resource. His justification isn’t very compelling but for demo-purposes I’ll choose to approve the request. 

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step4-17.png)

Once the request has been approved by the manager, return to the browser/chrome-profile of the requester and navigate to the Okta SSO dashboard. A new set of applications should have appeared on the requester's dashboard as a result of the access request process. If you had configured time-bound access, then access to these resources should automatically be revoked once the configured time has elapsed. 

## Conclusion 

We’ve seen how easy it is to set up various access request flows. We made one that automatically approves access and assigns the user the relevant resource(s). We also made another request flow where the user requested access to a oktagroupph giving him access to whatever access that oktagroupph should have. We also showcased how we can bake logic into the request flows by only running certain actions based on some conditions. And lastly we saw how easy it is to create time-bound access.

This concludes the *request access* portion of the lab.

For more information please head over to our [public documentation on OIG](https://help.okta.com/en-us/Content/Topics/identity-governance/iga.htm).


