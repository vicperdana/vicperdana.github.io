---
title: "Tips for Founders: setup your Azure account the right way"
excerpt_separator: "<!--more-->"
layout: single
toc: true
classes: wide
categories:
  - account
  - tenant
tags:
  - azure
  - foundation
---

As founders, your focus is to launch your product to the market as soon as possible.  Who don't? In this market, revenue/profit driven strategies are paramount, and cloud makes this a reality. 

Often, we encountered founders who created accounts a few years back and once they built, incubated, and launched their products to their market, they have to re-structure their Azure access and move off from their consumer email addresses e.g., Outlook, Hotmail, Gmail accounts as they expand their market penetration via Microsoft Commercial marketplace. And whilst there is nothing preventing anyone from creating their first subscription this way, you may want to re-consider and think about how you plan to use the subscription for production use to ensure seamless transition.

This post is targeted for start-up founders who are setting up an Azure subscription for the first time, gives you tips to set up your tenancy right from the get-go, more especially as you take advantage of commercial marketplace in Azure.  Who doesn't, with fee as low as 3% and ["80% acquisition expected to be made via digital channels"](https://www.gartner.com/en/newsroom/press-releases/2020-09-15-gartner-says-80--of-b2b-sales-interactions-between-su), you don't want to miss out. 

Bring in the cloud... :cloud: :sunny:
![the cloud](/assets/images/cloud.webp){: width="1000"}

# The basics 
Let's start with the basics and understand some key terminologies when setting up new subscriptions. 

## Subscription
Azure offers the ability to set up subscriptions so you can be billed for only the resources you use. There are many types of subscriptions in Azure - Pay-As-You-Go subscriptions are best for customers or partners who want the flexibility to pay for only the resources you use and is commonly chosen when setting up Azure subscription for the first time.

## Azure AD
When you create a subscription for the first time, you will also create Azure Active Directory (AD), when you go to azure.com and click "Sign up" - you will be directed to a wizard that provisions your subscription, enter in those juicy payment details information and as part of this subscription creation, Azure will automatically create a new Azure Active Directory with the name of "Default Directory".  Will discuss shortly why it is often *not a good idea* to use this tenant for production use - note: will refer this as *original* Azure AD tenant.

![AD Tenant](/assets/images/defaultadtenant.jpeg){: width="800"}

### Personal vs. Work account
A *personal* Microsoft account is an account that you use for your personal, non-work-related purposes. This might include using services like Outlook.com for your personal email or using OneDrive to store and access your personal files. A *work* Microsoft account, on the other hand, is an account that is provided to you by your employer and is intended for use in connection with your job. This account might give you access to additional services and resources that are provided by your employer, such as a corporate email account or the ability to access your company's intranet. In the context of this blog post is on the use of Microsoft Partner Center, as this service requires you to use a work account. 

# The tips
Here are some tips to sign up to Azure the right way from the get-go which should make future transition easier as you leverage Microsoft commercial marketplace via Partner Center.  

## Tip #1: Do not use the "Default Directory" Azure AD Tenant
This is where most founders may not realise that the AAD tenant they are using is derived from their personal email address! For example: "joeblogsoutlook.onmicrosoft.com".  You may not want to use this for building your production resources so I recommend ["creating a new AD tenant"](https://docs.microsoft.com/en-us/azure/active-directory/develop/howto-create-new-tenant). 

## Tip #2: Add your domain to the new Azure AD tenant
When you start a new company, you would have the name of your company and have secured a domain name from a domain name registrar - I would recommend ["adding this domain"](https://docs.microsoft.com/en-us/azure/active-directory/fundamentals/add-custom-domain) to your newly created Azure Active Directory.

## Tip #3: Create a new user account with your own domain
Create a new user account and assign it with your domain name and follow ["security best practice"](https://learn.microsoft.com/en-us/azure/active-directory/roles/security-planning) when assigning admin roles.

## Tip #4: Transfer your subscription to the new Azure AD tenant
Since the subscription is assigned to the original Azure AD tenant, you want to ["move the subscription to the new tenant"](https://learn.microsoft.com/en-us/azure/role-based-access-control/transfer-subscription).  Note: I would strongly recommend doing an impact assessment if you have any existing resources in your current subscription.

## Tip #5: (optional) if no longer required, remove your original Azure AD tenant
To keep it simple, remove your original Azure AD tenant (the one that was automatically created) including your personal e.g., outlook.com; user account.

# Ok, what's next
Hopefully, the tips above will help you establish an incredible Azure journey :muscle: and there are many resources that can help you build a solid foundation as follows.  

Note: the following is a non-exhaustive list of recommendations, notably the Cloud Adoption Framework, which is an amazing resource that you must keep at your disposal as you grow and build more exciting things in Azure. 

["Admin and Billing Account"](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ready/landing-zone/design-area/azure-billing-ad-tenant)<br>
["Landing zone for small-medium scale enterprises"](https://github.com/Azure/Enterprise-Scale/blob/main/docs/reference/treyresearch/README.md)<br>
["Azure AD Security best practices"](https://learn.microsoft.com/en-us/azure/security/fundamentals/identity-management-best-practices)<br>

# Oops...I haven't done any of the above and have built everything under my "Default Directory" Azure AD tenant and need access to Microsoft Partner Center
Please keep an eye on another blog post suggesting ways to remediate this and get yourself selling in Marketplace as soon as possible. Till next time :raised_hands:

**Disclaimer: The ideas, opinions, and recommendations expressed in this blog post are those of the author only, and do not necessarily reflect the views of the author’s employer or any other organization. In no event shall the author(s) be liable for any claims or damages or other liability arising from the use of advice given in this blog**