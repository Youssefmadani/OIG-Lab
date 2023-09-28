# Okta Identity Governance - Access Requests 
## Creating your first Access Request flow

Now, on to the exciting stuff! We will start by creating an access request flow that is automatically approved, allowing end-users to request access to the Salesforce dummy application we added to our environment in the earlier section of the lab. From the **Access Requests admin UI**, navigate to Access Requests and **click** on the **Create request type** button on the top right.  

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step3-1.png)

**Fill in** the name, description, select the team we just created and make this resource requestable for everyone by choosing **Everyone** at the Audience level. **Click** on **Continue**. 

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step3-2.png)

Notice that we've set the Audience  to everyone at [Org Name]. This means that everyone in my org can request the resource. We could limit this to specific groups and only make the resource requestable by them. But for the purposes of the lab we will just select *everyone*. 

This request flow will be very simple since we won’t ask for justification and won't require approval. This resource can be requested by everyone and access will be auto-approved. This ensures end-users can gain immediate access to the tools they need to work. This also helps us from a license-cost saving perspective. By not provisioning the application as a birth-right application, we avoid provisioning the app to users who won't end up using the resource.

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step3-3.png)

The request flow will only consist of one Action, so select the last option and choose **Assign Individual app to User**.

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step3-4.png)

You should be presented with the screen that you see in the screenshot above. The entire flow consists of 1 task which we can name on the right hand side of the UI.  Make sure that it is a required task and also **enable** Run automatically? This allows us to select the application we want to assign.

Once we enable **Run automatically** we should see two more drop downs. *Email Address* and *Select the application*. Configure the dropdowns like the screenshot below.

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step3-5.png)

Once that is done, on the top right click on **Publish**.

Now it’s time to check whether this Request flow works as expected. We haven’t configured SSO or Provisioning for any of the dummy applications since that is out of scope. But we want end users to still see the resource, request it and have the request be auto approved.

## Testing the access-request flow from an end-user's perspective

So let’s sign in as an end-user and log into the Okta end-user dashboard. (**tip**: Use another browser/chrome profile to sign in as the end-user. This way you won't have to log in/out as the admin/user whenever you want to do some testing).

Navigate to your Okta sign in dashboard and sign in with the user credentials below:

```
username: david.hume@oig.rocks
password: OktaRocks123!
```

David should only have 1 application in his dashboard which is the **Request Access** application.  **Click** on the application at which point David will be redirected to the access request portal. 

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step3-6.png)

The end-user should be presented with an App Catalog with one requestable resource, which is *Salesforce (auto-approved)*. Click on **Request Access** and then on **Submit new request**. 

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step3-7.png)

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step3-8.png)

You will see the progress of the request in the next screen which should result in an auto-approve and the message that the resource can be accessed from the Okta dashboard. 

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step3-9.png)

If we now navigate to David's Okta End User dashboard, we should see the Salesforce application on his SSO dashboard.

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step3-10.png)

This was a very simple example of creating an Access Request flow through an individual app-assignment that is automatically approved and immediately provisioned. We haven't talked about adding a time-limit to resource assignment. Neither have we mentioned how approvals, logic and other actions can be baked into an access request flow. We will cover that in the next section.