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
 
 Go back to the new Role and edit the assignment to tie the **Okta Access Requests OAuth** application to the new resource set you created above.

 ![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step4-y.png)

 Once you save this, the Access Requests app will be able to see all Okta Workflows you specified in the resource set.

#### Further Configure Okta Access Requests

Now we need to navigate back to the Access Requests dashboard. Navigate from the **Okta admin dashboard** -- to --> **Identity Governance** --> *Access Requests* (opens in a new tab) and from there go to **Settings --> Resources**:

 ![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step4-z.png)

 ![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step4-aa.png)

 Now click on **Manage Access** (see screenshot above 4.) and allow the existing teams to use the Workflows as resources in Access Request flows.

  ![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step4-ab.png)

  If you don't see the **ACCESS REQUEST - Application Admin Role** we just created in the previous steps, click on **Update Now** and refresh the page after 10-20 seconds. This will occur normally on the 24-hour refresh cycle, but you can request an update immediately (it will run in the background).

   ![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step4-ac.png)



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

Navigate to the Okta login portal and log in with David Hume's credentials.

```
username: david.hume@okta.rocks
password: OktaRocks123!
```

Notice on the top right, that David does not have Admin privileges. If he were to have such privileges, we would see an Admin button on the top right of the end-user dashboard. We will now request access to the Application-admin role as David.

**Click** on the **Okta Access Requests** Application. 

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step4-12a.png)

This should open up the *App Catalog*. David should see two requestable resources. The *Application Admin Role*, and the *Salesforce (auto-approved)* application. Under *Application Admin Role* **click** on **Request access**. 

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step4-12b.png)

 Fill in the justification field, specify a date until when David requires access, and submit your request. 

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step4-12c.png)

A new task is automatically created that can be followed by David, the approver(s) and the admins. We can for example see that manager approval is required. We made John Wick the manager of David Hume in the earlier section of the lab. And we see that reflected in the Request feed.

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step4-12d.png)


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

### Approving Access as John Wick

Now if we log into the access requests dashboard as the manager (John Wick), we should see that there is a new task pending. Open up a new Chrome-profile, an in-cognito tab or log out as David Hume. Now sign into your tenant with John Wick's Credentials:

```
username: john.wick@okta.rocks
password: OktaRocks123!
```
Notice that John Wick is also not an admin. However, he can still approve/deny access requests as a manager through the Access Requests console (or through one of the methods discussed above, i.e. Slack/Team/e-mail).

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step4-16.png)

Go ahead and **click** on the *Okta Access Requests* application as John. John should now be presented with the same App Catalog that we saw earlier. But we don't want to request access. We want to see what requests we need to approve/deny as the manager. So go ahead and **click** on *Requests* on the top left. *(PS. Normally the manager would get an email notification or a notification through a chat application whenever he/she needs to review an access request).*

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step4-17.png)

We see 1 open request that needs action from John Wick. Go ahead and **click** on the request.

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step4-18.png)

Take a moment to see the information that the manager has on this feed.

1. We can see information of the requester.
2. The Resource that is being requested access to.
3. The business justification of the requester.
4. The requested end-date (until when has David requested access to this resource)

**Note**: *We don't restrict the requester when he/she is choosing an end-date. They can set the end-date to one year from now. The policy however specifies that they cannot get access to app-admin privileges for more than 10 days. So the Workflow that's being kicked off as a result of approval will make sure that access is granted for either 10 days or the end-user requested end-date. Whichever one is shorter.*

Go ahead and **click** on *Approve*.

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step4-19.png)

Once we approve the access request as John Wick, we can see that the next action in the flow is performed:
*Temporarily provision app-admin privileges (max 10 days).*

**David Hume should now have been added to the Application-admins group as a result of the workflow that is triggered by the manager approval.**

### Enjoying our new app-admin privileges as David Hume

Sign back in as David Hume using the following credentials:
```
username: david.hume@okta.rocks
password: OktaRocks123!
```
Notice that he now has access to the Admin dashboard as an Application admin. **Click** on *Admin* in the top right.

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step4-20.png)

David Hume can now perform Application Admin tasks with his new-found privileges. Notice also that he is limited with regards to what he can do as an admin. Many permissions and menu-items are invisible to him because his admin-privileges are tightly scoped to one of the default admin roles.

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step4-21.png)


### Curious about the Workflow that was triggered?

If you would like to know how the Workflow is executed based on manager approval, Sign back in as a super-admin in Okta, go to the **Okta Admin Dashboard** and open up the **Workflow console**.

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step4-22.png)

From the **Workflows console** go to Flows and **click** on the Delegated flow we created earlier.

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step4-23.png)

Now **click** on *Execution History* and from the right-hand side-bar choose the first item.

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step4-24.png)

Notice that the delegated flow takes information from the access request submission (such as business justification, endDate etc.). This is then fed into the remainder of the flow. Being able to see this execution history in real time is also great for troubleshooting unexpected behaviors. Feel free to explore this page by scrolling to the right.

Once the specified time (maximum 10 days) have passed, the last card in the flow will automatically remove David Hume from the Application-admins group. Causing him to lose all of his app-admin priviliges.

## Conclusion 

We’ve seen how easy it is to set up various access request flows. We made one that automatically approves access and assigns the requester to Salesforce. We also made another request flow where the user requested access to an application-admin role. This access request flow required Manager approval. Upon approval a custom Workflow (automation) was triggered to ensure that the user is granted the right permissions for no more than a maximum of 10 days.

This concludes the *request access* portion of the lab.

### External Resources

For more information on **Okta Identity Governance**, head over to our [public documentation](https://help.okta.com/en-us/Content/Topics/identity-governance/iga.htm).

If you are curious on how **Custom Admin Roles** work in Okta. Check out [this page](https://help.okta.com/oie/en-us/content/topics/security/custom-admin-role/custom-admin-roles.htm).

For more information on **Okta Workflows**, check out [this page](https://help.okta.com/wf/en-us/content/topics/workflows/workflows-main.htm).