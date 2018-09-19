---
title: Best practices for enterprises moving to Azure
description: Describes a scaffold that enterprises can use to ensure a secure and manageable environment.
author: rdendtler
ms.date: 03/31/2017
ms.author: rodend;karlku;tomfitz
---
# Azure enterprise scaffold - prescriptive subscription governance

Enterprises are increasingly adopting the public cloud for its agility and flexibility. They are utilizing the cloud's strengths to generate revenue or optimize resources for the business. Microsoft Azure provides a multitude of services that enterprises assemble like building blocks to address a wide array of workloads and applications.

Deciding to use Microsoft Azure is only the first step to achieving the benefit of the Cloud. The second step is understanding how the enterprise can effectively use Azure and identify the baseline capabilities that need to be in place to address questions like:

* "I'm concerned about data sovereignty, how can I ensure that my data and systems meet our regulatory requirements?"
* "How do I know what every resource is supporting so I can account for it and bill it back accurately?"
* "I want to make sure that everything we deploy or do in the Public Cloud starts with the mindset of security first, how do I help facilitate that?"

The prospect of an empty subscription with no guard rails is daunting. This blank space can hamper your move to Azure.

This article provides a starting point for technical professionals to address the need for governance, and balance it with the need for agility. It introduces the concept of an enterprise scaffold that guides organizations in implementing and managing their Azure environments in a secure way. It provides the framework to develop effective and efficient controls.

## Need for governance

When moving to Azure, you must address the topic of governance early to ensure the successful use of the cloud within the enterprise. Unfortunately, the time and bureaucracy of creating a comprehensive governance system means some business groups go directly to vendors without involving enterprise IT. This approach can leave the enterprise open to compromise if the resources are not properly managed. The characteristics of the public cloud - agility, flexibility, and consumption-based pricing - are important to business groups that need to quickly meet the demands of customers (both internal and external). But, enterprise IT needs to ensure that data and systems are effectively protected.

In real life, scaffolding is used to create the basis of a structure. The scaffold guides the general outline, and provides anchor points for more permanent systems to be mounted. An enterprise scaffold is the same: a set of flexible controls and Azure capabilities that provide structure to the environment, and anchors for services built on the public cloud. It provides the builders (IT and business groups) a foundation to create and attach new services keeping speed of delivery in mind.

The scaffold is based on practices we have gathered from many engagements with clients of various sizes. Those clients range from small organizations developing solutions in the cloud to large multi-national enterprises and independent software vendors who are migrating and developing solutions in the cloud. The enterprise scaffold is "purpose-built" to be flexible to support both traditional IT workloads and agile workloads; such as, developers creating software-as-a-service (SaaS) applications based on Azure platform capabilities.

The enterprise scaffold is intended to be the foundation of each new subscription within Azure. It enables administrators to ensure workloads meet the minimum governance requirements of an organization without preventing business groups and developers from quickly meeting their own goals. Our experience shows that this greatly speeds, rather than impedes, Public Cloud growth.

> [!IMPORTANT]
> Governance is crucial to the success of Azure. This article targets the technical implementation of an enterprise scaffold but only touches on the broader process and relationships between the components. Policy governance flows from the top down and is determined by what the business wants to achieve. Naturally, the creation of a governance model for Azure includes representatives from IT, but more importantly it should have strong representation from business group leaders, and security and risk management. In the end, an enterprise scaffold is about mitigating business risk to facilitate an organization's mission and objectives.
>

The following image describes the components of the scaffold. The foundation relies on a solid plan for the management hierarchy and subscriptions. The pillars consist of Resource Manager policies and strong naming standards. The rest of the scaffold comes from core Azure capabilities and features that enable and connect a secure and manageable environment.

![enterprise scaffold](./_images/scaffoldv2.png)

## Define your hierarchy

The foundation of the scaffold is the hierarchy and relationship of the Azure Enterprise Enrollment through to subscriptions and resource groups. The enterprise enrollment defines the shape and use of Azure services within your company from a contractual point of view. Within the enterprise agreement, you can further subdivide the environment into departments, accounts, and finally, subscriptions and resource groups to match your organization structure.

![hierarchy](./_images/agreement.png)

An Azure subscription is the basic unit where all resources are contained. It also defines several limits within Azure, such as number of cores, virtual networks and other resources. Azure Resource Groups are used to further refine the subscription model and enable a more natural grouping of resources.

Every enterprise is different and the hierarchy in the above image allows for significant flexibility in how Azure is organized within the company. Modeling your hierarchy to reflect the needs of the company for billing, resource management and resource access is the most import -and first- decision you make when starting in the public cloud.

### Departments and Accounts

The three common patterns for Azure Enrollments are:

* The **functional** pattern

    ![functional](./_images/functional.png)
* The **business unit** pattern

    ![business](./_images/business.png)
* The **geographic** pattern

    ![geographic](./_images/geographic.png)

Though each of these patterns has it's place, the **business unit** patterns is increasingly being adopted for it's flexibility in modeling an organizations cost model as well as reflecting span of control. Microsoft Core Engineering and Operations group has created a sub-set of the **business unit** unit pattern that is very effective, modeled on **Federal**, **State**, and **Local** (see this [link](https://azure.microsoft.com/en-us/blog/organizing-subscriptions-and-resource-groups-within-the-enterprise/)).

### Management Groups

Microsoft has recently released a preview of a new way of modeling your hierarchy: Management Groups ([link](https://docs.microsoft.com/en-us/azure/azure-resource-manager/management-groups-overview)). Management groups are much more flexible than departments and accounts and can be nested up to six levels. Management Groups allow you to create a hierarchy that is separate from your billing hierarchy, solely for efficient management of resources. While Management Groups can mirror your billing hierarchy -and often enterprises start that way- the power of the management group is when you use it to model your organization where common subscriptions -regardless where they are in the billing hierarchy -are grouped together to provide common RBAC, Policies & Initiatives and other capabilities over time.  A few examples

* **Production/Non-Production** - Some enterprises create management groups to identify their production and non-production subscriptions. Management groups allow these customers to more easily manage roles and policies, for example: non-production subscription may allow developers "contributor" access, but in production, they have only "reader" access.
* **Internal Services/External Services** - much like Production/Non-Production, enterprises often have different requirements, policies and roles for internal services vs external (customer facing) services.

Well thought out management groups are, along with Azure Policy and Initiatives is a backbone of your efficient governance of the public cloud.

### Subscriptions

When deciding on your Departments and Accounts (or management groups), you are primarily looking at how you're dividing up your Azure environment to match your organization. Subscriptions, however, are where the real work happens and your decisions here impact security, scalability and billing.  Many organizations look at the three following patterns as their guides

* **Application/Service**:  Subscriptions are representative of an application or a service (portfolio of applications)
* **Life-cycle**:  Subscriptions are representative of a life-cycle of a service, such as Production or Development.
* **Department**: Subscriptions are representative of departments in the organization.

Of the three patterns above, the first two are the most commonly used patterns and are both highly recommended. The Lifecycle approach is appropriate for most organizations and in this case, the general recommendation is to utilize two base subscriptions "Production" and "Non-Production" and then utilize Resource Groups to break out the environments further.

### Resource group

Azure Resource Manager enables you to put resources into meaningful groups for management, billing, or natural affinity. Resource groups are containers of resources that have a common life cycle or share an attribute such as "all SQL servers" or "Application A".

Resource groups can't be nested and resources can only belong to one resource group. You can apply certain actions on all resources in a resource group. For example, deleting a resource group removes all resources within the resource group. Like subscriptions, there are common patterns when creating resource groups and will vary from "Traditional IT" workloads to "Agile IT" workloads:

* "Traditional IT" workloads are most commonly grouped by items within the same life cycle, such as an application. Grouping by application allows for individual application management.
* "Agile IT" workloads tend to focus on external customer-facing cloud applications. The resource groups often  reflect the layers of deployment (such as Web Tier, App Tier) and management.

> [!NOTE]
> Understanding your workload helps you develop a resource >group strategy. These patterns can be mixed and matched as well, for example, shared services type of resource group in the same subscription as "Agile" resource groups.

## Naming standards

The first pillar of the scaffold is a consistent naming standard. Well-designed naming standards enable you to identify resources in the portal, on a bill, and within scripts. You likely already have existing naming standards for on-premises infrastructure. When adding Azure to your environment, you should extend those naming standards to your Azure resources.

> [!TIP]
> For naming conventions:
> * Review and adopt where possible the [Patterns and Practices guidance](../guidance/guidance-naming-conventions.md). This guidance helps you decide on a meaningful naming standard and provides extensive examples.
> * Using Resource Manager Policies to help enforce naming standards
>
>Remember that it's difficult to change names later, so a few minutes now will save you trouble later.

Note, concentrate your naming standards on those resources that are more commonly used and searched for.  For example, all resource groups should follow a strong standard for clarity.

### Resource Tags

Resource tags are tightly aligned with naming standards. As resources are added to subscriptions, it becomes increasingly important to logically categorize them for billing, management and operational purposes. Tagging resources is covered in the article [Use tags to organize your Azure Resource](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-using-tags).
> [!NOTE]
> Tags can contain personal information and may fall under the regulations of GDPR. Plan for management of your tags carefully. If you're looking for general info about GDPR, see the GDPR section of the [Service Trust Portal](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted).

Tags can be used in many ways beyond billing and management. Many people use resource tags as part of automation (see later section). This can cause conflicts if not considered up front. The recommended practice is to identify all the common tags at the enterprise level (such as ApplicationOwner, CostCenter) and apply them consistently when deploying resources (or through other means). Applying them in the same order also helps later when parsing them as part of cost management.

## Azure Policy and Initiatives

The second pillar of the scaffold involves using [Azure Policy and Initiatives](https://docs.microsoft.com/en-us/azure/azure-policy/azure-policy-introduction) to manage risk by enforcing rules (with effects) over the resources and services in your subscriptions. Azure Initiatives are collections of policies that are designed to achieve a single goal. Azure policy and initiatives are then assigned to a resource scope to begin enforcement of the particular policies.
<IMG of Initiatives/Policies/Assignments>
> [!NOTE]
> Azure Policy and Initiatives are even more powerful when used with with the Management groups we mentioned earlier.  Management groups enable the assignment of an initiative or policy to an entire set of subscriptions.

### Common uses of Resource Manager policies

Azure policies and initiatives are a powerful tool in the Azure toolkit. Policies allow companies to provide controls for "Traditional IT" workloads that enable the stability that is needed for LOB applications while also allowing "Agile" workloads; such as, developing customer applications without opening up the enterprise to additional risk. The most common patterns we see for policies are:

* **Geo-compliance/data sovereignty** - Azure has an ever growing list of regions across the world. Enterprises often need to ensure that resources in a particular scope remain in a specific region to address regulatory requirements.
* **Avoid exposing servers publicly** - Azure policy can prohibit the use of certain resources types. A common use is to create a policy to deny the creation of a public IP within the particular scope, avoiding un-intended exposure of a server to the internet.
* **Cost Management and Metadata** - Resource tags are often used to add important billing data to resources and resource groups such as CostCenter, Owner and more. These tags are invaluable for accurate billing and management of resources. Policies can enforce the application of resources tags to all deployed resource, making it easier to manage.

### Common uses of initiatives

The introduction of initiatives provided enterprises to group logical policies together and track as a whole. Initiatives further support the enterprise to address the needs of both "agile" and "traditional" workloads. We have seen very creative uses of initiatives, but commonly we see:

* **Enable monitoring in Azure Security Center** - this is a default initiative in the Azure Policy and an excellent example of what initiative are. It enables policies that identify un-encrypted SQL databases, VM vulnerabilities and more common security related needs.
* **Regulatory specific initiative** - Enterprises often group policies common to a regulatory requirement (such as HIPAA) so that controls and compliancy to those controls are tracked efficiently.
* **example 3** - Think of another common one

> [!TIP]
> We recommend that you always use initiative definitions instead of policy definitions. After assigning an initiative to a scope, such as subscription, you can easily add another policy to the initiative without having to change any assignments. This makes understanding what is applied and tracking compliance far easier.

### Policy and Initiative assignments

After the creation of policies and grouping them into logical initiatives you must assign the policy to a scope, whether it is a management group, a subscription or even a resource group. Assignments allow you to also exclude a sub-scope from the assignment of a policy. For example, if you deny the creation of public ips within a subscription, you could create an assignment with an exclusion for a resource group connected to your protected dmz.

>[!NOTE]
> You can find a number of example Policy samples that show how Policy and Initiatives can be applied to various resources within Azure on this [GitHub](https://https://github.com/Azure/azure-policy) repository.

## Identity and Access Management

One of the first, and most crucial, questions you ask yourself when starting with the public cloud is "who should have access to resources?" and "how do I control this access?" Allowing or disallowing access to the Azure portal, and controlling access to resources in the portal is critical to the long term success and safety of your assets in the cloud.

To accomplish the task of securing access to your resources you will first configure your "Identity Provider" and then configure the Roles and access. Azure Active Directory (AAD), connected to your on-premises Active Directory, is the foundation of Azure Identity. That said, AAD is NOT Active Directory and it's important to understand what an AAD tenant is and how it relates to your Azure enrollment.  Review the available [information](https://docs.microsoft.com/en-us/azure/architecture/cloud-adoption-guide/adoption-intro/tenant-explainer) to gain a solid undersanding of AAD and AD. To connect and synchronize your Active Directory to Azure Active Directory, you will install and configure the [AD Connect tool](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect) on-premises.

![arch.png](./_images/arch.png)

When Azure was initially released, access controls to a subscription were basic: Administrator or Co-Administrator. Access to a subscription in the Classic model implied access to all the resources in the portal. This lack of fine-grained control led to the proliferation of subscriptions to provide a level of reasonable access control for an Azure Enrollment. This proliferation of subscriptions is no longer needed. With role-based access control (RBAC), you can assign users to standard roles that provide common access such as "owner" or "reader" or create your own roles

When implementing roles based access, keep the following tips:

* Control the Administrator/Co-Administrator of a subscription as these roles have extensive permissions. You only need to add the Subscription Owner as a Co-administrator if they need to managed Azure Classic deployments.

* Use Management Groups to assign [roles](https://docs.microsoft.com/en-us/azure/azure-resource-manager/management-groups-overview#management-group-access)across multiple subscriptions and reduce the burden of managing them at the subscription level.

* Add Azure users to a group (for example, Application X Owners) in Active Directory. Use the synced group to provide group members the appropriate rights to manage the resource group containing the application.
* Follow the principle of granting the **least privilege** required to do the expected work.

> [!NOTE]
>Consider using [Azure AD Privileged Identity Management](https://docs.microsoft.com/en-us/azure/active-directory/privileged-identity-management/pim-configure), Azure [Multi-Factor Authentication](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfa-getstarted) and [Conditional Access](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-conditional-access-azure-portal) capabilities to provide better security and more visibility to administrative actions across your Azure subscript. These capabilities come from a valid Azure AD Premium license (depending on the feature) to further secure and manage your identity. AAD PIM enables on-demand "Just-in-Time" administrative access with approval workflow, as well as a full audit of administrator activations and activities. Azure MFA is another critical capability which enables two-step verification for login to the Azure portal. When combined with Conditional Access Controls you can effectively manage your risk of compromise.

Planning and preparing for your identity and access controls and following Azure Identity Management best practice ([link](https://docs.microsoft.com/en-us/azure/security/azure-security-identity-management-best-practices)) is one of the best risk mitigation strategies that you can employ and should be considered mandatory for every deployment.

## Security

One of the biggest blockers to cloud adoption traditionally has been the concerns over security. IT risk managers and security departments need to ensure that resources in Azure are protected and secure by default. Azure provides a number of capabilities that you can leverage to protect resources, and detect/prevent threats against those resources.

### Azure Security Center

The [Azure Security Center](https://docs.microsoft.com/en-us/azure/security-center/security-center-intro) provides a unified view of the security status of resources across your environment in addition to advanced threat protection. Azure Security Center is an open platform that enables Microsoft partners and independent software vendors to create software that plugs into Azure Security Center to enhance its capabilities. The baseline (free tier) provides insight to your resources via policy and provides assessment and recommendations that will enhance your security posture.

Azure Security Center has a range of capabilities and price points from a free tier to paid tiers that enable additional and valuable capabilities such as Just In Time admin access and adaptive application controls (whitelisting).

> [!TIP] Azure security center is a very powerful toolset that is constantly being enhanced, incorporating new capabilities you can leverage to  detect any threats and protect your enterprise. We strongly advise to using this capability by itself, or along with existing tools, to enable a more secure environment.

### Azure resource locks

As your organization adds core services to subscriptions it becomes increasingly important to ensure disruption to availability to avoid business disruption. One type of disruption that we often see is unintended consequences of scripts and tools working against an Azure subscription deleting resources mistakenly. [Resource Locks](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-lock-resources) enable you to restrict operations on high-value resources where modifying or deleting them would have a significant impact on your applications or cloud infrastructure. You can apply locks to a subscription, resource group,or even an individual resource. Typically, you apply locks to foundational resources such as virtual networks, gateways, and storage accounts.

> [!TIP]
> Protecting Core network options should be the first area where you use resource locks. Accidental deletion of a gateway, security group or ExpressRoute circuit would cause serious issues for workloads. While Azure will prevent you from deleting a virtual network that is in use, additional controls around other network related objects are very useful. Examples:
>
> * Virtual Network: CanNotDelete
> * Network Security Group: CanNotDelete
> You can apply similar locks to Application security groups and other controls.

### Secure DevOps Toolkit

The "Secure DevOps Kit for Azure" (AzSK) is a collection of scripts, tools, extensions, automations, etc. originally created by Microsoft's IT Team and released in OpenSource via Github ([link](https://https://github.com/azsk/DevOpsKit-docs)). AzSK caters to the end to end Azure subscription and resource security needs for teams using extensive automation and smoothly integrating security into native dev ops workflows helping accomplish secure dev ops with these 6 focus areas:

* Secure the subscription
* Enable secure development
* Integrate security into CICD
* Continuous Assurance
* Alerting & Monitoring
* Cloud Risk Governance

![Azure dev ops Toolkit](_images/Secure_DevOps_Kit_Azure.png)

The AzSK is a rich set of tools, scripts and information that are an important part of a full Azure governance plan and incorporating this into your scaffold is crucial to supporting your organizations risk management goals

Summary

## Monitor and Alerts

Intro

Karl to Add Monitor content here.

Azure Monitor
Log Analytics (?)
Create Alerts
TIP: Top Alerts

Give a tip on alerting. Perhaps around action groups.

## Cost Management

One of the major changes that you will face when you move from on-premises cloud to the public cloud is the switch from capital expenditures (buying hardware) to operating expenditure (paying for service as you use it). This switch from CAPEX to OPEX also brings the need to more carefully manage your costs. The benefit of the cloud is that you can fundamentally and positively impact the cost of a service you use by merely turning it off (or resizing) when it's not needed. The flip side is that you can inadvertently  spend beyond your intent if you are not careful. Deliberately  managing your costs in the cloud is a recommended practice and one that mature customers do on a daily basis.

Microsoft provides a large number of tools for you to be able to visual, track and manage your costs. We also provide a full set of APIs to enable you to customize and integrate cost management into your own tools and dashboards.  These tools are loosely grouped into: Azure Portal Capabilities and external capabilities

### Azure Portal Capabilities

These are tools to provide you instant information on cost as well as the ability to take actions

* **Subscription Resource Cost**: Located in The Portal, the [Azure Cost Analysis](https://docs.microsoft.com/en-us/azure/cost-management/overview) view provides a quick look at your costs and information on daily spend by resource or resource group.

* **Azure Cost Management** : This product is the result of the purchase of Cloudyn by Microsoft and allows you to manage and analyze your Azure spend as well what you spend on other Public Cloud providers. There are both free and paid tiers, with a great wealth of capabilities as seen in the [overview](https://docs.microsoft.com/en-us/azure/cost-management/overview).

* **Azure Budgets and Action Groups** Knowing what somethings costs and doing something about it until recently has been more of a manual exercise. With the introduction of Azure Budgets and it's APIs, it's now possible to create actions (as seen in [this](https://channel9.msdn.com/Shows/Azure-Friday/Managing-costs-with-the-Azure-Budgets-API-and-Action-Groups) example) when costs hit a threshold. For example, shutting down a "test" resource group when it hits 100% of it's budget, or [another example].

* **Azure Advisor** Knowing what something costs is only half the battle, the other half is knowing what to do with that information. [Azure Advisor](https://docs.microsoft.com/en-us/azure/advisor/advisor-overview) provides you recommendations on actions to take to save money, improve reliability or even increase security.

### External Cost Management Tools

* **PowerBI Azure Consumption Insights** - Do you want to create your own visualizations for your organization? If so, then the Azure Consumption Insights content pack for PowerBI is your tool of choice. Using this content pack and PowerBI you can create custom visualizations to represent your organization, do deeper analysis on costs and add in other data sources for further enrichment.
* **Consumption API** - The [consumption APIs](https://docs.microsoft.com/en-us/rest/api/consumption/) give you programmatic access to cost and usage data in addition to information on budgets, reserved instances and marketplace charges. These APIs are accessible only for Enterprise Enrollments and some Web Direct subscriptions however they give you the ability to integrate your cost data into your own tools and data warehouses. You can also access these APIs by using the Azure CLI, seen [here](https://docs.microsoft.com/en-us/cli/azure/consumption?view=azure-cli-latest).

> [!NOTE]
> When we look across customers who have used the cloud for a long time and are "mature" in their use, we see a number of highly recommended practices
>* **Actively Monitor Costs** - Organizations that are mature Azure users constantly monitor costs and take actions when needed. Some organizations even dedicate people to do analysis and suggest changes to usage, and these people more than pay for themselves the first time they find an unused HDInsight cluster that's been running for months.
>* **Use Reserved Instances** - Another key tenant for managing costs in the cloud is to use the right tool for the job. If you have an IaaS VM that must stay on 24x7, then using a Reserved Instance will save you significant money. Finding the right balance between automating the shutdown of VMs and using RIs takes experience and analysis.
>* **Use Automation effectively** - Many workloads do not need to be running every day. Even turning off a VM for a 4 hour period every day can save you 15% of your cost. Automation will pay for itself quickly.
>* **Use Resource Tags for visibility** - As mentioned elsewhere in this document, using Resource Tags will allow for better analysis of costs.
>
Cost management is a discipline that is core to the effective and efficient running of a public cloud. Enterprises that achieve success will be able to control their costs and match them to their actual demand as opposed to overbuying and hoping demand comes.

## Automate

Intro
Azure Automation
Update Management (TIP?)
LogicApps&Functions
Top 5 Recommendations

## Templates & DevOps

Intro
Templates
Azure CLI

## Core Network

Access to resources can be either internal (within the corporation's network) or external (through the internet). The final component of the Azure scaffold reference model is core to how your organization accesses Azure, in a secure manner. It is easy for users in your organization to inadvertently put resources in the wrong spot, and potentially open them to malicious access. As with on-premises devices, enterprises must add appropriate controls to ensure that Azure users make the right decisions. For subscription governance, we identify core resources that provide basic control of access. The core resources consist of:

* **Virtual networks** are container objects for subnets. Though not strictly necessary, it is often used when connecting applications to internal corporate resources.
* **Network security groups** are similar to a firewall and provide rules for how a resource can "talk" over the network. They provide granular control over how/if a subnet (or virtual machine) can connect to the Internet or other subnets in the same virtual network.

![core networking](./media/resource-manager-subscription-governance/core-network.png)
Intro
VDC (Virtual Data Center)
VNET
ASG
NSG
Azure FW (NOTE)

## Next steps

* Now that you have learned about subscription governance, it's time to see these recommendations in practice. See [Examples of implementing Azure subscription governance](azure-scaffold-examples.md).

> [!div class="nextstepaction"]
> [Implement an example](azure-scaffold-examples.md)