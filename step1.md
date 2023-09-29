# Okta Identity Governance - an Introduction

## Introduction

During the workshop we will attempt to familiarize you with Okta Identity Governance or OIG for short. Our Governance solution can be thought of as consisting of three modules. Okta Identity Governance (Access Requests/Reviews/Reporting), Okta Lifecycle management and Okta Workflows. We will mainly focus on OIG and time-permitting also go into Okta Workflows. We are assuming that participants are familiar with our automated provisioning capabilities and will just be working with dummy Applications in favor of focussing on our Governance capabilities.

## Check if OIG is enabled for your tenant

First things first. Navigate to your **Okta Admin dashboard** and ensure that Okta Identity Governance is activated for your tenant. On the left side-bar you should see an item named Identity Governance. If not, reach out to your instructor and he/she should be able to activate it for you.

 ![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step1-1.png)

## Check if you have some dummy users / groups / applications

For the lab we will be leveraging the automatically created users, groups and applications. We won't configure the application(s) for SSO or provisioning but still need these resources to explain how access requests and certifications work in Okta Identity Governance (OIG).

### Checking our User Directory (Universal Directory)

From the **Okta Admin Dashboard**, navigate to **Directory --> People**. 

 ![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step1-2.png)

If you only see yourself, (that means that the automation portion of this lab hasn't been finished yet. In that case, go ahead and) **click** on the **Add person** button and create the following users. Make sure that you **enable** **Activate now**, check **I will set Password**, and uncheck **User must change password on first login**  :

- **First name, Last name, Username, primary email, pw = password**
1. John, Wick, john.wick@okta.rocks, john.wick@okta.rocks, pw = OktaRocks123!
2. David, Hume, david.hume@okta.rocks, david.hume@okta.rocks, pw = OktaRocks123!

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step1-3.png)

Go ahead and **click** on David Hume.

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step1-4.png)

 From here, navigate to the **Profile** tab, click on **Edit** and *scroll all the way down to the Manager attribute*. 

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step1-5.png)

David will be managed by John Wick. So in the Manager field, we will type in John's username, which -if you followed the above instructions - should be john.wick@okta.rocks. Go ahead and click on Save afterwards.

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step1-6.png)

Normally this sort of information will be sourced from an HR system or a separate directory (AD/LDAP). But for the sake of the demo we will just do this in Okta.

### Creating the right Okta Groups

From the **Okta Admin dashboard**, navigate to **Directory --> Groups**. If you only see the two default Okta groups (Everyone and Administrators), (that means that the automation portion of this lab hasn't been finished yet. In that case, go ahead and) Add the following Groups:

1. Application-admins
2. Marketing
3. Sales

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step1-7.png)

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step1-8.png)

### Assigning an Admin role to an Okta Group

We will explain why this is done later in the lab. But for now, know that we have default admin roles in Okta. We also have the ability to create custom admin roles (we will see that later in the lab). But the idea is that anyone who ends up in the Okta Application-admins group will inherit the Application Admin role. We will make this a requestable resource in the later portion of the lab.

From the **Okta Admin Dashboard**, navigate to **Security --> Administrators** and go to the **Admins** tab. **Click** on **Add administrator**.

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step1-9.png)

Under *Select admin*, find the **Application-admins** Okta group (that we just created). Under *Complete the assignment*, select **Application Administrator** as the role (this is one of the default admin roles that we have). We have the ability to limit this role to specified apps, but for the sake of the demo we can just keep the default value. Click on Save Changes. Now anyone who will be put into the Application-admins group, will inherit the permission-set of the Application Administrator role.

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step1-10.png)

### Creating a dummy application which will serve as a requestable Resource in Okta Identity Governance

From the **Okta Admin Dashboard**, navigate to **Applications --> Applications** and **click** on **Browse App Catalog**.

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step1-11.png)

  This brings us to the Okta Integration Network or the OIN for short. Let's add the Salesforce application to our demo environment. **Click** on the **Salesforce.com** application. 

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step1-12.png)


**Click** on **+ Add Integration**.

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step1-13.png)

  In the **General Settings** tab, scroll all the way down and click Next, and in the following **Sign-on Options** tab also scroll down and click **Done**. (no configuration required).

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step1-14.png)

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step1-15.png)

