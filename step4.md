# Creating a request flow with logic, an approver and a workflows integration

Now we will create a request flow that does require approval from the manager. We could have multiple steps of approval, for example, only assign the resource if both the manager and the group owner approve the request. But in this case manager approval will suffice. We will also incorporate some logic into the flow which will enable us to only perform certain tasks if others have been completed. And then finally, upon manager approval, we will trigger a specific workflow which will enable us to grant the requestor elevated permissions for the requested time-frame.

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
username: bruce.wayne@oktane.com
password: OIGR*cks!
```

Click on the **Request Access** Application. Bruce should see two requestable resources to him. An applicationph and an oktagroupph. The first flow was created earlier during the lab and is automatically approved upon request. (*If Bruce already has access to a resource it might not be displayed on this screen*). We want to test the second flow so under **Oktagroupph** click on **Request Access**. Fill in the justification field with some text and submit your request. A new task is automatically created that can be followed by Bruce, the approver(s) and admins. We can for example see that manager approval is required. 

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


