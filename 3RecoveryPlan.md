
# Recovery Plans for ASR

Recovery plan can be simple or complex according to the Virtual machines footprint in the platform or application and team's appetite for automation. A complex recovery plan can have multiple groups each groups comprising a set of Virtual machines. Each group can have pre and post actions that can be either manual or automated tasks with the help of runbooks. 

Now all this can be overwhelming if the configuration is maintained as parameters in Json files. Similar to pain points in ASR onboarding, achieving a right balance between logic and flexible/parameters will be challenging. A preprocessing logic is again employed that can build the complex JSON required for ARM template deployment. 

# Building a Preprocessing Logic

The CSV file comprising VMs and Group we used for ASR onboarding along with a CSV comprising group actions will be consumed by the preprocessing script to build the recovery plan

![image](https://user-images.githubusercontent.com/86707819/140833960-027377e3-aac4-4c6e-a5f3-f02252222aa6.png)

![image](https://user-images.githubusercontent.com/86707819/140833879-9c296dd2-f8c9-4abf-9536-f308c3381433.png)

https://github.com/aravindsundaram/AzureSiteRecovery/blob/main/Scripts/RecoveryPlanProcessing.ps1

**Script Logic:**
- Script iterates the VM CSV files for the recovery plan chosen to be processed. 
- Identifies the number of unique groups 
- Build the groups of VMs by referencing the Pre and Post actions in group actions CSV file. 
- It is not mandatory to have pre and post actions for every group of machines. 

Sample Group comprising single VM with pre and post actions. 

![image](https://user-images.githubusercontent.com/86707819/140847823-d8090002-8823-4a31-a638-9185d1ae5c44.png)

![image](https://user-images.githubusercontent.com/86707819/140847957-df8108de-809e-457f-b257-0dd2e42dbdef.png)
# CSV Layout

| Field | Description  |
|--|--|
| recoveryPlan | Name of Recovery Plan without "RecoveryPlan" prefix |
| group | Group Number |
| startAction | Start Action for the Group. Accepted values are Manual, Runbook  |
| startActionName  | Name of Start Action |
| startActionDescription | Description of Start Action |
| endAction | End Action for the Group. Accepted values are Manual, Runbook |
| endActionName | Name of End Action |
| endActionDescription | Description of End Action |
| failoverType| Type of Failover. Accepted value are TestFailover,UnplannedFailover |
| failoverDirections| Direction of Failover. Accepted values are PrimaryToRecovery, RecoveryToPrimary |

# Github Workflow

[![ASR-RecoveryPlan](https://github.com/aravindsundaram/AzureSiteRecovery/actions/workflows/ASR-RecoveryPlan.yml/badge.svg)](https://github.com/aravindsundaram/AzureSiteRecovery/actions/workflows/ASR-RecoveryPlan.yml)
