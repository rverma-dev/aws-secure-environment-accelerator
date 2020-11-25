| TASK                                                                                                   | Accelerator - What happens, WHERE, under what condition, on each state machine execution                                                                                                                                      |
| ------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **_ Creates AWS Account _**                                                                            |                                                                                                                                                                                                                               |
| - create mandatory accounts                                                                            | once per account, global scope                                                                                                                                                                                                |
| - create workload accounts                                                                             | once per account, global scope                                                                                                                                                                                                |
| - AWS Organizations Account and OU activities (OU or account rename, move account between OU's, etc.)  | once per organization, global scope                                                                                                                                                                                           |
| - Performs 'account warming' to establish initial limits, when required                                | state Machine region only (per region potential)                                                                                                                                                                              |
| - Checks limit increases, when required (complies with initial limits until increased)                 | per account, per region (supported limits only)                                                                                                                                                                               |
| - Automatically submits limit increases, when required                                                 | state Machine region only (per region potential)                                                                                                                                                                              |
| **_ Creates Networking _**                                                                             |                                                                                                                                                                                                                               |
| - Transit Gateways and TGW route tables                                                                | in the defined region(s), defined account(s)                                                                                                                                                                                  |
| - Centralized and/or Local (bespoke) VPC's                                                             | in the defined region(s), defined account(s)                                                                                                                                                                                  |
| - Subnets, Route tables, NACLs, Security groups, NATGWs, IGWs, VGWs, CGWs                              | part of any VPC, in the defined region(s), defined account(s)                                                                                                                                                                 |
| - Deletes default VPC's (worldwide)                                                                    | in all regions, in all accounts, can disable regions (all accounts or specific account)                                                                                                                                       |
| - VPC Endpoints (Gateway and Interface)                                                                | part of any VPC, in the defined region(s), defined account(s)                                                                                                                                                                 |
| - use central endpoints                                                                                | configures regional central endpoints (one 'central' VPC per region)                                                                                                                                                          |
| - Route 53 Private and Public Zones                                                                    | in the defined account(s), defined region(s), defined VPC(s), global scope                                                                                                                                                    |
| - Resolver Rules and Resolver (inbound/outbound) Endpoints                                             | part of a specific VPC(s), in the defined region(s), defined account(s) (i.e. per region possible)                                                                                                                            |
| ...including MAD R53 DNS resolver rule creation                                                        | created in same region as MAD only, shared to same region VPC's when use-central-endpoints set                                                                                                                                |
| - R53 VPC Endpoint Overloaded Zones                                                                    | same region(s), same account(s) as the endpoint and VPC(s)                                                                                                                                                                    |
| **_ Cross-Account Object Sharing _**                                                                   |                                                                                                                                                                                                                               |
| - VPC and Subnet sharing, including account level retagging (Per account security group 'replication') | VPC's are shared to accounts within the SAME REGION as the source VPC only<br>An OU could have additional VPC's defined for additional regions and would be shared to the appropriate accounts in the same additional regions |
| - VPC attachments and peering (local and cross-account)                                                | in the defined region, no cross-region attachments or peering supported                                                                                                                                                       |
| - Managed Active Directory sharing                                                                     | state machine region only (consider same region as the MAD only)                                                                                                                                                              |
| - Automated TGW inter-region peering                                                                   | cross-region, cross-account or same-account                                                                                                                                                                                   |
| **_ Zone sharing and VPC associations _**                                                              |                                                                                                                                                                                                                               |
| Public Hosted Zones                                                                                    | no sharing, no association required                                                                                                                                                                                           |
| Private Hosted Zones - CLOUD DNS DOMAIN                                                                | associate worldwide, all VPC with use-central-endpoints                                                                                                                                                                       |
| Endpoint Private Hosted Zones                                                                          | associate within region, for all VPC use-central-endpoints                                                                                                                                                                    |
| On-premise resolver rules                                                                              | associate within region, for all VPC use-central-endpoints                                                                                                                                                                    |
| MAD resolver rule association                                                                          | same region as the MAD resolver only, assoc. w/all VPC use-central-endpoints                                                                                                                                                  |
| **_ Identity _**                                                                                       |                                                                                                                                                                                                                               |
| - Creates Directory services (Managed Active Directory and Active Directory Connectors)                | in a specific VPC, in the defined region, defined account - only 1 per account, therfore can't have a second region in the same account                                                                                       |
| - Creates Windows admin bastion host auto-scaling group                                                | once per above MAD (once per account), same region as MAD                                                                                                                                                                     |
| - Set Windows domain password policies (initial installation only)                                     | once per above MAD (once per account), same region as MAD                                                                                                                                                                     |
| - Set IAM account password policies                                                                    | once per account, global scope                                                                                                                                                                                                |
| - Creates Windows domain users and groups (initial installation only)                                  | once per above MAD (once per account), same region as MAD                                                                                                                                                                     |
| - Creates IAM Policies, Roles, Users, and Groups                                                       | once per account, global scope                                                                                                                                                                                                |
| **_ Cloud Security Services _**                                                                        |                                                                                                                                                                                                                               |
| - Enables and configs the following AWS services, worldwide w/central specified admin account:         | (each service can have specified regions disabled)                                                                                                                                                                            |
| - Guardduty (roadmap to add new S3 capabilities)                                                       | enabled all regions, all accounts, admin account per region                                                                                                                                                                   |
| - Security Hub (Enables specified security standards, and disables specified individual controls)      | enabled all regions, all accounts, admin account per region                                                                                                                                                                   |
| - Firewall Manager                                                                                     | enabled once per account (global scope), single admin account                                                                                                                                                                 |
| - CloudTrail w/Insights and S3 data plane logging                                                      | enabled all regions (using Organization trail, stored in Organization Management account)                                                                                                                                     |
| - Config Recorders/Aggregator                                                                          | enabled all regions, all regions include global events, aggregator set to specified reion in Organization Management account                                                                                                  |
| - Macie                                                                                                | enabled all regions, admin account per region                                                                                                                                                                                 |
| - IAM Access Analyzer                                                                                  | enabled once per account (global scope), single admin account                                                                                                                                                                 |
| - CloudWatch access from central specified admin account                                               | enabled once per account (global scope), Two admin accounts (Ops & Security)                                                                                                                                                  |
| **_ Other Security Capabilities _**                                                                    |                                                                                                                                                                                                                               |
| - Creates, deploys and applies Service Control Policies                                                | once per organization, global scope                                                                                                                                                                                           |
| - Creates Customer Managed KMS Keys (SSM, EBS, S3)                                                     | SSM and EBS keys are created if a VPC exists in the region, S3 if we need an Accel bucket in the region, per account                                                                                                          |
| - Enables account level default EBS encryption                                                         | set if a VPC exists in the region, per account                                                                                                                                                                                |
| - S3 Block Public Access                                                                               | once per account, global scope                                                                                                                                                                                                |
| - Configures Systems Manager Session Manager w/KMS encryption and centralized logging                  | set if a VPC exists in the region, per account                                                                                                                                                                                |
| - Creates and configures AWS budgets (customizable per OU and per account)                             | once per account, global scope                                                                                                                                                                                                |
| - Imports or requests certificates into AWS Certificate Manager                                        | State Machine region only (per region potential, required for alb deployments)                                                                                                                                                |
| - Deploys both perimeter and account level ALB's w/Lambda health checks, certs & TLS policies          | State Machine region only (per region potential)                                                                                                                                                                              |
| - Deploys & configures 3rd party firewall clusters and management instances                            | in the defined region(s), defined account(s)                                                                                                                                                                                  |
| **_ Centralized Logging _**                                                                            |                                                                                                                                                                                                                               |
| - Deploys an rsyslog auto-scaling cluster behind an NLB, all syslogs forwarded to CWL                  | State Machine region only (per region potential)                                                                                                                                                                              |
| - Centralizes logging to a single centralized S3 bucket (enables, configures and centralizes) incl:    | N/A                                                                                                                                                                                                                           |
| - VPC Flow logs (w/Enhanced metadata fields and optional CWL destination)                              | part of a specific VPC, in the defined region, defined account (to local account bucket in state machine region, replicated to log-archive primary region)                                                                    |
| - Organizational Cost and Usage Reports                                                                | once per organization, global scope (to local account bucket in state machine region, replicated to log-archive primary region)                                                                                               |
| - CloudTrail Logs including S3 Data Plane Logs (also sent to CWL)                                      | directly back to log-archive, specified primary region                                                                                                                                                                        |
| - All CloudWatch Logs (includes rsyslog logs) (and setting Log group retentions)                       | State machine region, plus configured regions                                                                                                                                                                                 |
| - Config History and Snapshots                                                                         | directly back to log-archive account specified primary region                                                                                                                                                                 |
| - Route 53 Public Zone Logs, DNS Query Logging                                                         | to CloudWatch Logs in us-east-1 (which are sent to S3)                                                                                                                                                                        |
| - GuardDuty Findings                                                                                   | directly back to log-archive, specified primary region                                                                                                                                                                        |
| - Macie Discovery results                                                                              | directly back to security, specified primary region, replicated to log-archive                                                                                                                                                |
| - ALB Logs                                                                                             | State Machine region only (same as ALB deployment)                                                                                                                                                                            |
| - SSM Session Logs                                                                                     | All regions currently send back to central region, log-archive account                                                                                                                                                        |

---

## Region support

- We have tested installing the Accelerator in multiple different primary regions, but have not done a formal region support assessment
- As most features can be toggled on/off (per region), we expect most regions should be supportable as a primary (or installation) region, and when not, it should be supported as a managed (or secondary) region
- The Accelerator cannot be _installed_ directly in the following regions due to a lack of Stack Set support: Africa (Cape Town), Asia Pacific (Osaka), China (Beijing), China (Ningxia), Europe (Milan), Middle East (Bahrain)
- We also expect to have issues _installing_ into regions which do not have Firewall Manager support, as we did not include the capability to disabled setting the admin account
- These regions are likely supported as managed but non-primary regions, the initial install simply must be in a fully supported region
- If sufficient demand exists, we may investigate resolving these blockers (Stack sets are only used for one trivial item today)

---

[...Return to Accelerator Table of Contents](./index.md)