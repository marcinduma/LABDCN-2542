# Nexus Dasboard Orchestrator Tenant configuration 

In this section you will configure your first Tenant, stretched between AWS and onprem.  

Following diagram shows what is to about to be configured. 

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/image49a.png" width = 800>

## Tenant Creation on NDO 

!!!Info
	Configuration will be fully deployed from ***Nexus Dashboard Orchestrator***. All activities - except AWS Trust - will be done at NDO GUI.


On the Left navigation page click **"Application Management" -> "Tenant"** and then **"Add Tenant"**

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/image50.png" width = 800>

Fill in Tenant details for name and description 

 - Display Name: **Tenant-01**
 - Descrption: **CL23 AMS Tenant-01**

Associate Tenant to both Sites by checking the checkbox next to it. 

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/image51.png" width = 800>

!!! Note 
    For now you are not able to **Save** this configuration with red marking on AWS Site. Click the **Pencil** button at the end of the site line to complete configuration. 

    Additional setting are needed for CNC, so it knows which Tenant ID to use on AWS. 

**cAPIC site configuration**

For AWS site we have 2 options - **Untrusted** with Cloud Access key and Secret or **Trusted**. In our case we would be using Trusted configuration. 

Each Tenant created on NDO associated to AWS Cloud required sepearete Account ID on AWS site. You can find your in POD Details under following Appendix (***OR ON RDP DESKTOP FILE*** to evaluate.)

Fill in with your POD ** AWS User-Account ID** and select **Access Type** as **Trusted**, hit **Save**. 

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/image53.png" width = 800>

Now configuration can be saved, leave assocaited user list empty as there are no additional users and hit **Save**.

On the Tenant list you should see **Tenant-01** created an assigned to 2 Sites. 

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/image54.png" width = 800>

In next step we will make AWS trust configuration, so CNC can make changes to AWS objects. 

You can find details about this process under **Resources** in **Appendixes** section of this Lab Guide. 