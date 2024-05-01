# AZ-305: Microsoft Azure Solutions Architect Certification Exam Notes

## Exam Sections

- Manage Azure identities and governance (15–20%)
  - Configure resource locks.
  - Manage resource groups.
- Implement and manage storage (15–20%)

- Deploy and manage Azure compute resources (20–25%)
  - Configure VM
    - Move VMs from one resource group to another.
- Configure and manage virtual networking (20-25%)
- Monitor and maintain Azure resources (10–15%)

### Resources

- Microsoft Learn Pathway: <https://learn.microsoft.com/en-us/credentials/certifications/azure-solutions-architect/>
- Exam prep videos:

  - [Preparing for AZ-305 - Design identity, governance, and monitoring solutions (1 of 4)](https://learn.microsoft.com/en-us/shows/exam-readiness-zone/preparing-for-az-305-design-identity-governance-and-monitoring-solutions-1-of-4)

  - [Preparing for AZ-305 - Design data storage solutions (2 of 4)](https://learn.microsoft.com/en-us/shows/exam-readiness-zone/preparing-for-az-305-design-data-storage-solutions-2-of-4)

  - [Preparing for AZ-305 - Design business continuity solutions (3 of 4)](https://learn.microsoft.com/en-us/shows/exam-readiness-zone/preparing-for-az-305-design-business-continuity-solutions-3-of-4)

  - [Preparing for AZ-305 - Design infrastructure solutions (4 of 4)](https://learn.microsoft.com/en-us/shows/exam-readiness-zone/preparing-for-az-305-design-infrastructure-solutions-4-of-4)

- [Youtube Playlist](https://www.youtube.com/watch?v=vq9LuCM4YP4&list=PLlVtbbG169nHSnaP4ae33yQUI3zcmP5nP)
- [Microsoft Sample Example](https://learn.microsoft.com/credentials/certifications/exams/az-305/practice/assessment?assessment-type=practice&assessmentId=15)
- [Study Guide](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/az-305)

# Design identity, governance, and monitoring solutions (25–30%)

## Overview

### Core Azure Structure

**Some of these need to be separated into other categories**

- Policy - What can you do
  - Policies can be grouped into initiatives
- RBAC - Who can do it
- Budget - How much

- Azure Blueprint
  - RG's
    - ARM JSON Templates
    - RBAC
    - Policy
  - Can be stored in Sub or Management group
    - Can be applied below that thing
  - Modes:
    - Don't lock - can adjust and delete
    - Don't delete - Can adjust but not delete
    - Read only

- Azure Domain Services
  - If we need to use LTLM or LDAP or Kerberos and we don't have a domain, we'd use Azure Domain Services
  - Creates a managed AD in a virtual network

- AAD Connect - Syncs on prem to cloud AAD (entra connect)

- B2B
  - Add a user as a guest
  - Use their Source provider (google, etc), and it does authentication on that source provider
  - AAD - does the authentication

- B2C (Buisiness to Consumer)
  - Customers can create a local account on the B2C
  - Or use any of a large number of social accounts to login

- Managed Identity
  - System MI
    - Tied with a particular resource
    - Lifecycle is tied directly with resource
  - User Assigned MI
    - Separate lifecycle from a resource
    - Own resource on the tenant
    - Can Grant x resources to use that MI
      - Then can just grant then MI to a permission to the thing that you are giving access to
    - Lifecycle is almost like 1:n
    - Allows multiple resources needing the same permissions to only need 1 MI
    - *EX Q*: You have 10 VM's that all need same set of permissions. What is the minimum number identities you can use
      - You only need 1 MI

- Key Vault
  - Stores:
    - Secrets
    - Keys
    - Certs
  - Now you can now give a MI read access to a secret as one example.
    - So you could have a resource that authenticates to MI, then the MI that can read the secret, and goes to other resources as needed

## Design solutions for logging and monitoring

### Monitoring

#### Data is generated from

- AAD
  - Auditing
  - Sign-in logs

- Subscription
  - Activity Log

- Resources
  - Metrics
  - Logs
    - These don't exist by default, have to be configured

- Others
  - Operating systems
  - Application
  - Insight capabilities, etc.

#### Data gets sent to

- Log Analytics Workspace
  - Duration of 2 years
- Event Hub
  - Publish/subscribe service
  - Good for third party
  - Can trigger other things like an azure function
- Storage Account
  - Is cheap retention

- Configuration
  - Driven by diagnostic Settings

- Alert Rules
  - Triggers:
    - Metrics
    - Log Analytics
    - Activity Log
  - Can raise Alerts
    - Alerts can fire Action Groups
      - Action Groups can do things like:
        - API
        - SMS
        - Function

## Design authentication and authorization solutions

## Design governance

# Design data storage solutions (20–25%)

# Design business continuity solutions (15–20%)

- Region
  - Has Fault and update domains
    - Fault domains is rack level
    - Update domain helps keep things online when updates happen
  - Availability Sets
    - Distributes work among racks, helps node and rack level failure
    - Create an availability Set for each unique workload
      - if you don't, by bad luck, you whole website may end up on one rack for example
  - Doesn't help building failures
    - Each Building has dedicated power, cooling, networking
  - Availability Zones
    - Logical separations per subscription, not necessarily building 1 is availabiliity zone 1
    - Creates resiliancy for a data center failure
  - Synchronous resiliancy for replication (less than 2ms)
  - Some services are Zone Redundant
    - Then that server is automatically distrubuted among multiple availabilty zones
  - Some services are Zonal
    - You choose which AZ your service lives in. So if you want redundancy, you need to make multiple services in different zones
  - If it's Regional
    - You don't know where in the region it is in
- Make sure you have equal resiliancy in all parts of your architecture/solution (otherwise you still have a less resiliant point of failure)
- Multiple region solutions
  - Is asynchronous - not synchronous, (can happen up to 15 min after sync?)

- Load Balancing
  - L7 - App Gateway
    - Lives in a region
  - L4 - Standard Load Balancer
    - Lives in a region
  - L7 solution - You can use Azure Front door to load balance between App Gateways
  - L4 - Another solution for not L7 - is traffic manager using DNS
    - Points to multiple load balancers
  - General rules:
    - Balance global solution with regional solution
    - Mix and match layers
      - Ex: If I have L7 App gateway, use L7 Front door in front

## Design solutions for backup and disaster recovery

### Backups

- What do I want to restore? (blob, whole db, etc?) How much data can I loose?

- Azure Backup
  - Has two modes:
  - Can act as an orchestrator
    - Ex: taking snapshots of a blob and keeping certain number of backups
    - Doesn't always make sense to backup blob snapshots (ex) into backup vault into same region
  - There are lots of settings
    - Ex: Azure backup does snapshots, but you can keep some instant snapshots locally for a period of time

- Data Solutions
  - Structured
    - Column/Rows
  - Semi-Structured
    - JSON/XML
  - Unstructured
    - Docs/Media
    - Storage account
      - BLOB (Binary large object)
        - Block
          - Access Tiers
            - Premium Storage Account
            - Standard Storage Account
              - Hot
              - Cool
              - Archived
                - Is Offline
        - Page
          - Good for random read writes
        - Append
      - Files
        - Primarily for SMB
        - NFS also works
      - Queues
        - FIFO solution
      - Types of storage accounts
        - Standard General Purpose V2
        - Premium
          - Types
            - Block
            - Files
            - Page
          - Generally high performance, but maybe have less options
            - No GRS options - only LRS or ZRS
      - Immuntability
        - Legal holds
        - Time based holds
    - Replication Options
      - LRS
        - 3 copies of data within one data cluster (building)
      - ZRS
        - 3 copies across data centers
      - GRS
        - 3 copies

## Design for high availability




# Design infrastructure solutions (30–35%)
