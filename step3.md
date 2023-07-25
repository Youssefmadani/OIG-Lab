# Creating your first Access Request flow

Now, on to the exciting stuff! We will start by creating an access request flow that is automatically approved, allowing end-users to request access to ApplicationPH. From the Access Requests admin UI, navigate to Access Requests and click on the Create request type button on the top right.  

![](https://github.com/Youssefmadani/OIG-Lab/tree/main/Images/
step3-1.png)

Fill in the name, description, select the team we just created and make this resource requestable for everyone by choosing Everyone at the Audience level. Click on Continue. 

![](https://github.com/Youssefmadani/OIG-Lab/tree/main/Images/
step3-2.png)

Notice that we've set the Audience equal to everyone at OIEZ (name of my org). This means that everyone in my org can request the resource. We could limit this to specific groups and only make the resource requestable by them. But for the purposes of the lab we will just select *everyone*. 

This request flow will be very simple since we won’t ask for justification or require approval. This resource can be requested by everyone and access will be auto approved. This ensures end users have the tools they need to work, but also doesn’t auto provision users for tools that can be costly but won’t end up being used by the end users. 

![](https://github.com/Youssefmadani/OIG-Lab/tree/main/Images/
step3-3.png)

The request flow will only consist of one Action, so select the last option and choose **Assign Individual app to User**.

![](https://github.com/Youssefmadani/OIG-Lab/tree/main/Images/
step3-4.png)

You should be presented with the screen that you see in the screenshot above. The entire flow consists of 1 task which we can name on the right hand side of the UI. Call it something like Assign Office 365 to user. Make sure that it is a required task and also **enable** Run automatically? This allows us to select the application we want to assign.
Once we enable **Run automatically** we should see two more drop downs. *Email Address* and *Select the application*. Configure the dropdowns like the screenshot below.

![](https://github.com/Youssefmadani/OIG-Lab/tree/main/Images/
step3-5.png)

Once that is done, on the top right click on **Publish**.

Now it’s time to check whether this Request flow works as expected. We haven’t configured SSO or Provisioning for any of the dummy applications since that is out of scope. But we want end users to still see the resource, request it and have the request be auto approved.

## Trying out the request access flow with an end-user

So let’s sign in as an end-user and log into the Okta end-user dashboard. (tip: Use another browser/chrome profile to sign in as the end-user. This way you won't have to log in/out as the admin/user whenever you want to do some testing).

Navigate to your Okta sign in dashboard and sign in with the user credentials below:

```
username: bruce.wayne@oktane.com
password: OIGR*cks!
```

I just logged in as my end-user. In your case he should only have 1 application in his dashboard which is the **Request Access** application.  Click on the application at which point Tom will be redirected to the access request portal. 

![](https://github.com/Youssefmadani/OIG-Lab/tree/main/Images/
step3-6.png)

The end-user should be presented with an App Catalog with one requestable resource, which is ApplicationPH. Click on **Request Access**. Click on **Submit new request**. 

![](https://github.com/Youssefmadani/OIG-Lab/tree/main/Images/
step3-7.png)

You will see the progress of the request in the next screen which should result in an auto-approve and the message that the resource can be accessed from the Okta dashboard. And sure enough, now that I refresh Tom’s Okta dashboard, it is populated with many office 365 apps. (Of course provisioning and SSO won’t work unless that has been set up, which is out of the scope of this lab). 

If you now navigate as an **Admin** to the Okta **admin dashboard → Applications → Applications** and find the **ApplicationPH app → Assignments** tab, you should also see that Bruce has been auto-assigned to this app directly. 

Notice that his assignment is an individual assignment since we assigned the app directly to the requester. We could also choose to assign the user to a group instead of an app. And have the app be assigned to the group. This can be useful when the organization decides that a specific role should have access to more resources. That way, you just add a resource to a group and all members of that group will be automatically granted access to those resources as well.