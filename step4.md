# Creating a request flow with an approver and logic

Now we will create a request flow that does require approval from the manager. We could have multiple steps of approval, for example, only assign the resource if both the manager and the application owner approve the request. But in this case manager approval will suffice.

From the Access Requests Admin UI navigate to Access Requests and click on **Create request type**.

![](https://github.com/Youssefmadani/OIG-Lab/tree/main/Images/
step4-1.png)

Type in the Name and give the request type a description. Notice the description mentions temporary access for 24 hours. We have the capability in OIG to automatically revoke access after a configurable amount of time has passed. Click on **Continue**.

![](https://github.com/Youssefmadani/OIG-Lab/tree/main/Images/
step4-2.png)

We’ll start off the flow with a question to the requester so choose **Question → Add to request type**.

![](https://github.com/Youssefmadani/OIG-Lab/tree/main/Images/
step4-3.png)

We made the question a required field so the requestor will have to answer before the flow continues. 

![](https://github.com/Youssefmadani/OIG-Lab/tree/main/Images/
step4-4.png)

Now let’s add another task which will be an approval task. At the bottom of the screen select **Approval**. 

![](https://github.com/Youssefmadani/OIG-Lab/tree/main/Images/
step4-5.png)

This task requires the manager of the requester to approve the request.  So let’s fill in the details. Name the task something like “*Manager approval required*”. Make it a required task. And Assigned to needs to be **Requester’s manager**. 

*Notice that we don't have to know **who** the manager of the requester is since this information lives in the requester's user profile (which has been configured automatically when this lab tenant was spun up by the demo-platform.) This profile information can be sourced from an HR system, a directory (AD/LDAP) or any other source (that exposes the necessary api's).*

![](https://github.com/Youssefmadani/OIG-Lab/tree/main/Images/step4-6.png)

Once the manager has given his/her approval we want to perform an Action as the next step in the flow. Again, at the bottom of the screen, click on Action to add another action/task to the flow. 

![](https://github.com/Youssefmadani/OIG-Lab/tree/main/Images/step4-8.png)

This time select **Add user to a group**.

![](https://github.com/Youssefmadani/OIG-Lab/tree/main/Images/step4-9.png)

Give the action a name such as *Adds user to marketing group/role*, make it required, and again enable *Run automatically?* The email address should be **requester email** and in the "*Select the group*" drop down select oktagroupph. This action simply adds the requester to a specific group. The group can represent a role or can simply be viewed as a selection of users. The group has resources and/or permission sets assigned to it, so all of these will be inherited by the requester.

[OPTIONAL] You might also want to *Add a time limit* to give the requester temporary access to the resource. (You can set this to a few minutes to see this in action during a demo).

![](https://github.com/Youssefmadani/OIG-Lab/tree/main/Images/step4-10.png)

## Adding logic to the flow

For the last step, we need to add some logic to ensure that the user is only assigned to the Marketing group upon Approval by the manager. Under **Tasks & Actions** click on the action named **Adds user to oktagroupph** if not already highlighted. Switch to the logic tab and configure it as shown below: 

```
Only show this task if
Manager approval Required
is approved
```

![](https://github.com/Youssefmadani/OIG-Lab/tree/main/Images/step4-11.png)

The end result should look like the image below.

![](https://github.com/Youssefmadani/OIG-Lab/tree/main/Images/step4-12.png)

Once finished, click on Publish at the top right to make the resource visible and requestable to the specified audience. 

## Testing if the access request flow works

*For this section we strongly recommend you open two additional Chrome profiles. One which functions as the browser of the requester and another one which functions as the approver (manager). This way you don't have to constantly log in/out. This is especially challenging since we're working with 3 user accounts (requestor, approver and admin).*

Navigate to the Okta login portal and log in with the following credentials.

```
username: bruce.wayne@oktane.com
password: OIGR*cks!
```

Click on the **Request Access** Application. Bruce should see two requestable resources to him. An applicationph and an oktagroupph. The first flow was created earlier during the lab and is automatically approved upon request. (*If Bruce already has access to a resource it might not be displayed on this screen*). We want to test the second flow so under **Oktagroupph** click on **Request Access**. Fill in the justification field with some text and submit your request. A new task is automatically created that can be followed by Bruce, the approver(s) and admins. We can for example see that manager approval is required. 

![](https://github.com/Youssefmadani/OIG-Lab/tree/main/Images/step4-13.png)

### 3 ways of approving access

An approver can approve requests in three ways. None of these ways require that the approver has admin access:

1. Through the request access console.
2. Via e-mail.
3. And through modern chat clients (Slack, Teams)

For the purposes of this lab we will be going with option number one. But to briefly show you how the other options might look like:

![](https://github.com/Youssefmadani/OIG-Lab/tree/main/Images/step4-14.png)
`This is how an access request might be approved/denied via e-mail.`

![](https://github.com/Youssefmadani/OIG-Lab/tree/main/Images/step4-15.png)
`This is how an access request might be approved/denied through Slack/Teams.`

Now if we log into the access requests dashboard as the manager, we should see that there is a new task pending. 

![](https://github.com/Youssefmadani/OIG-Lab/tree/main/Images/step4-16.png)

Clicking on the request reveals details such as why the requester requires access to this resource. His justification isn’t very compelling but for demo-purposes I’ll choose to approve the request. 

![](https://github.com/Youssefmadani/OIG-Lab/tree/main/Images/step4-17.png)

Once the request has been approved by the manager, return to the browser/chrome-profile of the requester and navigate to the Okta SSO dashboard. A new set of applications should have appeared on the requester's dashboard as a result of the access request process. If you had configured time-bound access, then access to these resources should automatically be revoked once the configured time has elapsed. 

## Conclusion 

We’ve seen how easy it is to set up various access request flows. We made one that automatically approves access and assigns the user the relevant resource(s). We also made another request flow where the user requested access to a oktagroupph giving him access to whatever access that oktagroupph should have. We also showcased how we can bake logic into the request flows by only running certain actions based on some conditions. And lastly we saw how easy it is to create time-bound access.

This concludes the *request access* portion of the lab.

For more information please head over to our public documentation on OIG:

https://help.okta.com/en-us/Content/Topics/identity-governance/iga.htm
