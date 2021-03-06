# ASR Onboarding

Some of common pain areas in ASR onboarding are,
- Configuration such as Disks, Specification, Resource Groups can be varying for every virtual machine.
- Lots of these details has to be supplied in form of Parameters files which can be really complex arrays/objects
- If extensive parameters arent supplied, building logic via ARM, Bicep or Terraform to fetch from VMs can be challenging and will be complex to write
- Maintaining Repository. Most of the foundations code comprises Recovery Services vault and teams generally do not mix up Non foundations components in it.

# Building a Preprocessing Logic

Below Azure Powershell script serves as a wrapper which does the majority of the preprocessing and enables replication for the VMs chosen. 

- feeds off a simple CSV file comprising Virtual Machines to be onboarded to ASR. 
- CSVs are easily configurable and maintainable.
- Script has capablity to read the specifications, disk details and formulate the details required for enabling replication. 
- Easier for infra teams to simply add VMs with minimal details such as replication policy,recovery plan and group numbers.

https://github.com/aravindsundaram/AzureSiteRecovery/blob/main/Scripts/EnableReplication.ps1

# CSV file layout

virtualMachinesDR.csv

| **Column Name** | **Description** |
|--|--|
| vmName | Name of the Virtual Machine |
| replicationPolicy | Name of the Replication Policy - Platinum,Gold,Silver,Bronze, NonProd depending upon RTO and RPO |
| recoveryPlan | Name of Recovery Plan. Can be platform or App based. Recovery plan when created will be prefixed with "RecoveryPlan" for consistency  |
| resourceGroup | Name of Resource Group |
| group | Sequence to be used in failover and failback. Accepted values are 1,2,3 etc i.e 1 is first group to failover followed by 2 which is 2nd group to failover |

![image](https://user-images.githubusercontent.com/86707819/139073328-8eb64b4e-311f-41ca-8417-9ea4a728bf0c.png)

# Github Workflow

A github workflow to perform ASR onboarding. 

https://github.com/aravindsundaram/AzureSiteRecovery/blob/main/.github/workflows/ASR-EnableReplication.yml

[![ASR-EnableReplication](https://github.com/aravindsundaram/AzureSiteRecovery/actions/workflows/ASR-EnableReplication.yml/badge.svg)](https://github.com/aravindsundaram/AzureSiteRecovery/actions/workflows/ASR-EnableReplication.yml)
