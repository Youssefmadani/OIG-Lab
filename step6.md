# Exploring Okta Identity Governance Reporting

The final capability we will explore is Reporting. The reporting engine in Okta has been rewritten to support the new governance reporting needs. New IGA reports are being added continuously.

This section of the document will explore some of the IGA reports.

The documentation to support this can be found at:

https://help.okta.com/en-us/content/topics/identity-governance/iga-reports.htm

## Accessing reports
Reporting can be found in the Okta Admin console:
- Log into the **Okta dashboard** as an administrator and go to **Admin**
- Go to **Reports > Reports**

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step6-1.png)

Whilst there are existing reports that may be relevant to IGA, there are two new sets of reports: Entitlements and Access, and Access Certification Campaigns reports. We will explore these below.

## Entitlement and Access Reports
We will have a look at some of these reports.
- From the **Reports** page select the **User App Access** report

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step6-2.png)

You are presented with an unfiltered view of all application assignments.

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step6-3.png)

The contents are self-explanatory, but the important information is how the app was assigned (Individual or Group) and if it’s a group assignment, what the group is, and how the group membership was done (Direct, i.e. manual, or By Rule, i.e. automatic).

` You might have to scroll horizontally to see other columns such as "Okta User Status" depending on the resolution of your screen.`

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step6-4.png)

As with all reports, you can apply filters or export the report as a CSV file for further analysis.

- Apply a filter to look for a specific application in your system

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step6-5.png)

- Go back to the **Reports** page
- Select the **Group Membership** report

As before, you get an unfiltered list of all users for each group. You can filter the report or download it as CSV.

## Access Certification Campaign Reports

Currently there are two reports for Access Certification Campaigns – a summary report and a detail report.

- From the **admin dashboard** go to the **Reports > Reports** page
- Select the **Past Campaign Summary** report

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step6-6.png)

The report shows a summary of all campaigns run, and for each information about the specific campaign. You can apply filters or download a CSV.

Whilst this is interesting, a more useful report is the campaign details report.

- Go back to the **Reports** page
- Select the **Past Campaign Details** report

This report shows all the campaign items for all campaigns.

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step6-7.png)

![](https://raw.githubusercontent.com/Youssefmadani/OIG-Lab/main/Images/step6-8.png)

In addition to the user and resource (i.e. application or group), for the review item it shows who the Reviewer was, the outcome (**Certification**), the date **(Certified), Business Justification** (if entered) and the results of the remediation (**Attempted remediation and Remediation status**).

Again, you could filter (say specific campaigns, users, reviewers or applications/groups) making this a very useful report. You can also download it as a CSV.

## Summary of Getting Started with Reporting

This completes the guided steps around reporting.

In this section we have walked through the some of the new IGA reports available with Okta Identity Governance and how they can be used.