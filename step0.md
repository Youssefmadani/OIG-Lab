# Getting started with Okta Identity Governance

 With the release of Okta Identity Governance we've introduced some new capabilities to the Okta platform. During this workshop we will attempt to familiarize you with how Okta does Governance. 
 
 ![](https://github.com/Youssefmadani/OIG-Lab/tree/main/Images/step0-1.png)

 ## What you will learn:

 - Creating various access request flows.
 - Creating certification campaigns to review access
 - Using our governance reporting capabilities

## Creating various access request flows.

In the first section of the lab we will be creating two access request flows. 

The first flow deals with requesting access to an application. This flow is automatically approved upon request. Some of you might ask: Why not just automatically assign that application to all users if everyone can request it with no approval required? It might turn out that only 60% of the assigned users will actually end up using that application. Meaning that 40% of the licensing costs will be wasted. Requiring users to actively request a resource instead of just giving them birthright access will improve the likelihood that user's assigned to the app are actually using the app. (There are other ways to dynamically check for unused apps and review or revoke access if an app is unused for x amount of days. But that is outside of the scope of this lab guide).

The second flow we will create does require approval. We will learn about the flexibility that we have when it comes to deciding who the approver should be. We will also learn about how we can bake logic into our access request flows and how to grant time-based access.

## Creating certification campaigns to review access

The next capability we will explore is Access Certification. Access Certification (aka attestation, recertification etc.) is how we validate that a user still needs the access to specific resources/permission-sets that they have. Certification campaigns may be run periodically, or there may be continuous certification when user roles change. We will learn about subjects like assigning dynamic reviewers, multi-level reviewers, remediation actions and more.

## Using our governance reporting capabilities

The final capability we will explore is Reporting. The reporting engine in Okta has been rewritten to support the new governance reporting needs. New IGA reports are being added continuously.
