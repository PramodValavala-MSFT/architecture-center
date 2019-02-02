---
title: "Fusion: Large Enterprise – Additional technical Details regarding Governance MVP"
description: Large Enterprise – Additional technical Details regarding Governance MVP
author: BrianBlanchard
ms.date: 2/1/2019
---

# Fusion: Large Enterprise – Additional technical Details regarding Governance MVP

The following outlines the implementation of the initial Corporate Policies for this Governance Journey, to support the initial narrative. Before implementation it is advised that the reader review, modify, and integrate those artifacts into decision making processes.

The core of this governance MVP is the [Deployment Acceleration](../../configuration-management/overview.md) discipline. The tools and patterns applied at this stage will enable the incremental evolutions needed to expand governance in the future.

[!INCLUDE [governance-mvp](../../../../../includes/cloud-adoption/governance/governance-mvp.md)]

## Policy enforcement

The first decision to make regarding Deployment Acceleration is the pattern for enforcement. In this narrative, the Cloud Governance team decided to implement the **Automated Enforcement** pattern.

- Azure Security Center will be made available to the security and identity teams to monitor security risks. Both teams are also likely to use Security Center to identify new risks and evolve corporate policy.
- RBAC is required in all subscriptions to govern authentication enforcement.
- Azure Policy will be published to each management group and applied to all subscriptions. However, the level of policies being enforced will be very limited in this initial governance MVP.
- Although Azure Management Groups are being used, a relatively simple hierarchy is expected.
- Azure Blueprints will be used to deploy and update subscriptions by applying RBAC requirements, Azure Resource Manager templates, and Azure Policy across management groups.

## Applying the dependent patterns

The following decisions represent the patterns to be enforced through the Policy Enforcement strategy above:

#### Identity Baseline 

Azure Blueprints will set RBAC requirements at a subscription level to ensure consistent identity is configured for all subscriptions.

#### Security Baseline: Software Defined Network

The Cloud Governance team maintains an Azure Resource Manager template for establishing a VPN gateway between Azure and the on-premises VPN device. When an application team requires a VPN connection, the Cloud Governance team will apply the gateway Resource Manager template via Azure Blueprints.

### Application of Governance Defined Patterns

The Cloud Governance team will be responsible for the following decisions and implementations. Many will require inputs from other teams, but the Cloud Governance team is likely to own both the decision and implementation. The following sections outline the decisions made for this use case and details of each decision.

#### Subscription Model

The **Mixed** pattern has been chosen for Azure subscriptions.

- As new requests for Azure resources arise, a "Department" should be established for each major business unit in each operating geography. Within each of the Departments, "Subscriptions" should be created for each application archetype.
- An application archetype is a means of grouping applications with similar needs. Common examples include: applications with protected data; governed applications (HIPAA, FedRAMP, etc.); low risk applications; applications with on-premises dependencies; SAP or other mainframes in Azure; or applications that extend on-premises SAP or mainframes. Each organization has unique needs based on data classifications and the types of applications that support the business. Dependency mapping of the digital estate can help define the application archetypes in an organization.
- A common naming convention should be agreed upon as part of the subscription design, based on the above two bullets.

#### Resource Consistency

**Hierarchical Consistency** has been chosen as a resource consistency pattern.

- Resource groups should be created for each application. Management groups should be created for each application archetype. Azure Policy should be applied to all subscriptions in the associated management group. 
- As part of the deployment process, Resource Management templates for all assets should be stored in source control. 
- Each resource group should align to a specific workload or application.
- The Azure Management Group hierarchy defined should represent billing responsibility and application ownership using nested groups.
- Extensive implementation of Azure Policy could exceed the team’s time commitments and may not provide much value at this point. However, a simple default policy should be created and applied to each resource group to enforce the first few cloud governance policy statements. This serves to define the implementation of specific governance requirements. Those implementations can then be applied across all deployed assets.

#### Resource Tagging

The **Accounting** pattern has been chosen for resource tagging.

- Deployed assets should be tagged with values for the following: Department/Billing Unit, Geography, Data Classification, Criticality, SLA, Environment, Application Archetype, Application, and Application Owner.
- These values along with the Azure Management Group and Subscription associated with a deployed asset will drive governance, operations, and security decisions.

#### Log & Reporting

At this point, a **Hybrid** pattern for log and reporting is suggested but not required of any development team.

- No governance requirements are currently set regarding the specific data points to be collected for logging or reporting purposes. This is specific to this fictional narrative and should be considered an antipattern. Logging standards should be determined and enforced as soon as possible.
- Additional analysis is required prior to the release of any protected data or mission critical workloads.
- Prior to supporting protected data or mission-critical workloads, the existing on-premises operational monitoring solution must be granted access to the workspace used for logging. Applications are required to meet security and logging requirements associated with the use of that tenant, if the application is to be supported with a defined SLA.

### Evolution of governance processes

Some of the policy statements cannot or should not be controlled by automated tooling. Other policies will require periodic effort from IT Security and on-premises Identity Baseline teams. The Cloud Governance team will need to oversee the following processes to implement the last eight policy statements:

**Corporate Policy Changes**: The Cloud Governance team will make changes to the Governance MVP design to adopt the new policies. The value of the Governance MVP is that it will allow for the automatic enforcement of the new policies.

**Adoption Acceleration**: The Cloud Governance team has been reviewing deployment scripts across multiple teams. They've maintained a set of scripts that serve as deployment templates. Those templates can be used by the cloud adoption teams and DevOps teams to more quickly define deployments. Each script contains the requirements for enforcing governance policies, and additional effort from cloud adoption engineers is not needed. As the curators of these scripts, they can implement policy changes more quickly. Additionally, they are viewed as accelerators of adoption. This ensures consistent deployments without strictly enforcing adherence.

**Engineer Training**: The Cloud Governance team offers bi-monthly training sessions and has created two videos for engineers. Both resources help engineers get up to speed quickly on the governance culture and how deployments are performed. The team is adding training assets to demonstrate the difference between production and non-production deployments, which helps engineers understand how the new policies affect adoption. This ensures consistent deployments without strictly enforcing adherence.

**Deployment Planning**: Prior to deployment of any asset containing protected data, the Cloud Governance team will be responsible for reviewing deployment scripts to validate governance alignment. Existing teams with previously approved deployments will be audited using programmatic tooling.

**Monthly Audit and Reporting**: Each month, the Cloud Governance team runs an audit of all cloud deployments to validate continued alignment to policy. When deviations are discovered, they are documented and shared with the cloud adoption teams. When enforcement doesn't risk a business interruption or data leak, the policies are automatically enforced. At the end of the audit, the Cloud Governance team compiles a report for the Cloud Strategy team and each cloud adoption team to communicate overall adherence to policy. The report is also stored for auditing and legal purposes.

**Quarterly Policy Review**: Each quarter, the Cloud Governance team and Cloud Strategy team to review audit results and suggest changes to corporate policy. Many of those suggestions are the result of continuous improvements and the observation of usage patterns. Approved policy changes are integrated into governance tooling during subsequent audit cycles.

## Alternative Patterns

If any of the patterns selected in this journey don't fit your requirements, consider the alternatives listed below.

- Subscription model: Alternatives to the **Mixed** pattern are available [here](../../../infrastructure/subscriptions/overview.md).
- Resource Consistency: Alternatives to the **Hierarchical  Consistency** pattern are available [here](../../../infrastructure/resource-grouping/overview.md).
- Resource Tagging: Alternatives to the **Accounting** pattern are available [here](../../../infrastructure/resource-tagging/overview.md).
- Identity: Alternatives to the **Directory Synchronization** pattern are available [here](../../../infrastructure/identity/overview.md).
- Software Defined Network: Alternatives to the **Cloud DMZ** pattern are available [here](../../../infrastructure/software-defined-networks/overview.md).
- Encryption: Alternatives to the **Cloud Native** pattern are available [here](../../../infrastructure/encryption/overview.md).
- Logging & Reporting: Alternatives to the **Hybrid** pattern are available [here](../../../infrastructure/log-and-reporting/overview.md).
- Enforcement Automation: Alternatives to the **Automated Enforcement** pattern are available [here](../../../infrastructure/policy-enforcement/overview.md).

## Next steps

Once this guide is implemented, the Cloud Adoption team can go forth with a sound governance foundation. The Cloud Governance team will work in parallel to continuously update the corporate policies and governance disciplines. 

The two teams will use the tolerance indicators to identify the next evolution needed to continue supporting cloud adoption. For the fictitious company in this journey, the next step is evolving this governance baseline to [support the inclusion of applications with legacy or third-party multifactor authentication (MFA) requirements](./identity-baseline.md).

> [!div class="nextstepaction"]
> [Identity baseline evolution](./identity-baseline.md)