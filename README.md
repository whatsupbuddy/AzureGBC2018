# Azure Recovery Services

Welcome! This repo has a bunch of content that will be getting covered in the Global Azure Bootcamp Auckland event for 2018. Here you can find the steps which we shall go through in the lab, the slide desk and some links to information hosted on technet, MVA and channel 9.

## Content

### What will you need for the lab?
1. A Microsoft Azure Account. You can sign up for a free trial [here](https://azure.microsoft.com/en-us/free/). - - If you dont have one already!
2. A computer running a Desktop OS such as WIndows, Mac or Linux with a browser such as Edge, Chrome or Firefox


### What you will learn

At the end of this session you should hopefully have a bit of insight into Azure Recovery Services and how you can leverage them for migration and DR purposes. An area we shall pay specific attention to is Azure Site Recovery, you should be leaving with an understanding around the approach you should be taking when considering ASR as a DR or Migration solution and some of the considerations to keep in mind when planning, prepping and executing.

During the following lab, we shall deploy a small environment consisting of a virtual machine, Vnet, storage accounts, and the components required for us to replicate / migrate the workloads. From here, we will enable replication of the VM to another region of Azure....


## Lab Steps

### Deploying VM Resources in Azure

First up, once you are logged into Azure, lets create the resources we need for this lab.

1. Select 'Create a resource' and look for 'Windows Server 2016 VM'

![alt text](/Images/1-CreateResource.png)

a. Complete Step 1 of New VM Wizard - Basic Info.

![alt text](/Images/2-VMstep1.png)

b. Complete Step 2 of New VM Wizard - Select a Size of your choice by clicking View All. D2s_V3 is a good base size.

![alt text](/Images/3-VMStep2.png)

c. Complete Step 3 of the New VM Wizard - Leave these settings as default as it should just create a new VNet for you. If you have an existing Vnet, just check this config and whether you are happy for the VM to join it.

![alt text](/Images/4-VMStep3.png)

d. Review summary and select 'Create'.

![alt text](/Images/5-VMStep4.png)

## ---------// Once this VM is deployed, if you wish to add more than a single VM to your environment, run through the above again and deploy another to same VNET \\---------




### Deploying Azure Recovery Services Vault

2. Select 'Create a resource' & search for 'Backup and Site Recovery (OMS)' 

![alt text](/Images/RecoveryVault-1.png)

3. When creating your vault, provide it a name, resource group and ensure its location is in a different region to where your VM was deployed.

###--------// IMPORTANT- If the vault is in the same region as your VM, you cannot use it \\---------

![alt text](/Images/RecoveryVault2.png)

![alt text](/Images/BEAWARE.png)



### Replicating 

Once all of your resources have provisioned, we can go ahead and enable replication on the VM to another location in Azure.

4. Locate your first virtual machine and go to Operations\Disaster Recovery (preview).

![alt text](/Images/locatevm.png)
![alt text](/Images/DROperation.png)

5. From here, Azure will populate all the fields below. Check over them to make sure there are no errors.
    - The Recovery vault you made in the previous step should be set by default. Click 'Enable Replication'

![alt text](/Images/configdr-vm.png)


Azure will now create your replication plan, resource group, vnet and start replicating your VM. It will do so by storing data in a storage cache account. This will take a few minutes to setup. Periodically check back to see the status.

While your waiting, feel free to go and get familiar with the settings available in your Recovery vault.

![alt text](/Images/Replicationstatus.png)



### Failover

Once you have 100 sync and protected status on your virtual machine, you can go ahead and either complete a test failover or a live failover. If you opt to do a failover prior to running a test one, Azure will pop up with a warning just to make you aware that you have not carried out a test failover yet.

![alt text](/Images/Replicationstatus.png)


6. Navigate to your Recovery Vault, then go to Protected Items\Replicated Items

![alt text](/Images/failover1.png)

7. Select the VM you wish to failover & click failover (if you choose test failover, just follow the azure blades, they will be similar to a real failover, you will just be required to choose your test vnet).

![alt text](/Images/failover2.png)

8. You will receive a warning like below if you are yet to do a test failover.

![alt text](/Images/failoverwarning.png)


9. Select your recovery point option. (last processed Low RTO may be the quickest).

![alt text](/Images/failover3.png)


10. Click Submit and your failover will be triggered. To view the jobs progress go back a blade to your Recovery Vault \ Monitoring and Reports\Jobs.

![alt text](/Images/jobs.png)

The failover will take some time, once it is finished you should see the VM running in your destination region. You will notice the other VM has been deallocated and stopped.

![alt text](/Images/redgreen.png)


As there is no Recovery plan or automation setup, the VM at the destination will not have a public ip address for you to access it via. Should you wish to login to the VM after the failover. You will need to create a new Public IP address and associate it with the VM.

If the VM is in a running state, Azure has finished the replication job.


### Well Done!!!

So you have successfully replicated workloads within Azure using ASR. The process you under took is similar to what you would go through when replicating workloads from on-prem. Your next steps would be to look into some of the content below, get familiar with the scenario specific to your environments and work through the plan // prep // execute approach of ASR.

The content will go into some Recovery plan designs and how you would use Azure Automation to orchestrate failover of your workloads.

Good Luck and thanks for following.



## Additional Content

- Channel 9 ASR Content - [Here](https://channel9.msdn.com/Series/Azure-Site-Recovery)
- Microsoft Tech ASR Content - [Here](https://docs.microsoft.com/en-us/azure/site-recovery/)
    - Replicating different workloads - [Here](https://docs.microsoft.com/en-us/azure/site-recovery/site-recovery-active-directory)
- Azure Site Recovery Deep Dive - [Here](https://channel9.msdn.com//Series/Azure-Site-Recovery/Azure-Site-Recovery-Deep-Dive/)
