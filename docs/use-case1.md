# Use-case 01 - Stretched VRF with EPG to EPG communication

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/uc-1.png" width = 800>

First use-case task will focus on:

- Configuration of stretched VRF inside Tenant-01
- VPC CIDR configuration in AWS
- EPG configuration in both sides 
- EC2 creation and EPG assigment
- Contract configuration to allow traffic flow 


## Schema, Template configuration 

All logical polices and configuration are done in Nexus Dashboard Orchestratod via Schemas and Templates. Each Schema can have multiple Templates, but Template can belong to only one Schema. Each Template is also assocaited to one and only one Tenant. Template to site assoction will also define to which fabric (on prem or cloud) configration will be pushed, therefore is the smallest logical unit we can decide where changes are deployed. 

### 1. Schema creation 

Navigate to **Dashboard -> Application Management -> Schemas**, then hit **"Add Schema"** 

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/image70.png" width = 800>

Add Schema name and Description and hit **"Add"** button

- Name: **Schema-T01** 
- Description: **Schema for Tenant-01**

### 2. Template creation 

Under the Schema-T01, let's create first Template with **"Add New Template"** button

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/image71.png" width = 800>

For a Template type select **"ACI Multi-Cloud"** and hit **"Add"**

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/image72.png" width = 800>

On the right side of the template screen, we can customize the Tempalte Display Name, and also select Tenant. Add Display Name and Select a Tenant. 

- Display Name: **temp-stretch-01**
- Tenant setting: **Tenant-01**

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/image73.png" width = 800>

### 3. Template to site association

For configuration in template to be deployed, appropriate sites need to be added. Sites added will decide to which fabric configuration will be pushed. 

For **temp-stretch-01**, we want to add both Azure and AWS sites. 

To do it under the **Template Properties** locate the **Actions** button and hit **Sites Associations**

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/image76.png" width = 800>

Select both site **on-prem** and **cAPIC** and hit **Ok**

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/image77.png" width = 800>

## VRF, EPG, selectors deployment  

### 1. VRF Configuration

Inside **Schema-T01**, inside **temp-stretch-01** add the VRF with **"Add VRF"** button under **VRFs** box

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/image74.png" width = 800>

Add VRF Common Properties 

- Display Name: **VRF-01** 
- Description: **CL2023 Stretch VRF**

Leave other as default. 

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/image75.png" width = 800>

As Tempalte was assocaited to both sites, we need to update details for each of them, to do so under **temp-stretch-01** expand **"Template Properties"** and go to **"cAPIC"** 

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/image78.png" width = 800>

Click on the **"VRF-01"** which opens **Site specific properties** for this **cAPIC Site**

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/image79.png" width = 800>

Under the **"Template Properties"** hit **"Add Region"** button and select **Region** from the list and then hit  **"Add CIDRs"** button 

 - **Region:** eu-central-1

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/image80.png" width = 800>

Now we need to specify what subnet we would like to use in AWS cloud for that VPC. This subnet will be configure as VPC CIDR on AWS cloud and will be used for VM and endpoint addressing. 

- CIDR: **10.0.0.0/23** 

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/image81.png" width = 800>

Our CIDR also needs to be divided into subnets for respective **Availability Zones (AZ)** - hit **"Add Subnet"** to configure it. 

We need to add subnet for a,b,c (number of AZ depends on the region), each subet needs to be confirmed with click on **checkbox** button.

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/image82.png" width = 800>

Create subnet for all 3 AZ in our Region, use non-virtual zone. 

 - **Subnet:** 10.0.0.0/25      **Name:** az-2a-subnet **AZ:** eu-west-2a
 - **Subnet:** 10.0.0.128/25    **Name:** az-2b-subnet **AZ:** eu-west-2b
 - **Subnet:** 10.0.1.0/25      **Name:** az-2c-subnet **AZ:** eu-west-2c


<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/image83.png" width = 800>

When done, hit **"Save"** button to save subnet configuration. 

We also want to connect our subnets to Catalys8000V routers, this is done via Hub Network (Transit Gateway in AWS) - check the checkbox for **"Hub Network"**, select **Hub Network** and also all add all **Subnets** created before. 

 - Hub Network: **TGW-HUB**
 - Subnets: **10.0.0.0/25, 10.0.0.128/25, 10.0.1.0/25** 

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/image84.png" width = 800>

Hit ok to finish configuration for **cAPIC** fabric.

!!!Info 

    AWS Transit Gateway(TGW) connects Amazon Virtual Private Clouds (VPCs) networks through a central hub. This connection simplifies network and puts an end to complex peering relationships. Transit Gateway acts as a highly scalable cloud router(don't be confused by naming - it's AWS internal cloud service and it's not related to Catalyst8000V routers which we also used call Cloud Routers). 

    In our case TGW are used to connect our **User VPC** with **Infrastructure VPC** where the Cloud Routers (Catalyst8000V) resides, this allows traffic flow between User Endpoints and Cloud Routes, so also for communication with another sites and also another AWS Regions. Selection of the subnet for Hub Network means that such subnet will be advertised to TGW, so traffic towards such subnet can flow. 


Under **temp-stretch-01** expand **"Template Properties"** and go to **"Template Properties"** main settings. 

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/image89.png" width = 800>

When done **"Deploy to sites"** button will become active (blue) - click to deploy VRF to respective sites. 

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/image90.png" width = 800>

Nexus Dashboard will also show what changes are to be made. Review those and hit **Deploy** button to push configuration. 

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/image91.png" width = 800>

If all goes well, confirmation pop-up will get displayed on the screen. 

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/image92.png" width = 800>

At this point in both on-prem and AWS cloud, there will be VRF/VPC created.

### 2. VRF verification 

In the browser, open AWS console page. From the Region list, make sure that you have London (eu-west-2) region selected. If not, switch to it. 

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/image94.png" width = 800>

Search for **"VPC"** resource in Search tool and open it. 

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/image93.png" width = 800>

You should have 2 VPCs created. 

- context-[VRF-01]-addr-[10.0.0.0/23] - coresponding to NDO created VRF 
- default, without name, created automaticaly by AWS

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/image95.png" width = 800>

Let's check now VRF on on-prem APIC.

Open APIC GUI and go to Tenants section.

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/uc-1-tenants.png" width = 800>

Double-click at Tenant-1 to enter configuration of your freshly created Tenant - using deploy at NDO.

Go to Networking -> VRFs to check if new VRF-01 is created.

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/uc-1-tenant-vrf-1.png" width = 800>

As you can see, creation of new Tenant and VRF from NDO has been done successfully. Let's continue with next steps.

### 3. Additional Template creation 

As per our topology diagram, EPGs in each Site are separate. As current **temp-stretch-01** is associated with both sites. We need to create two(2) new templates in the **Schema-T01** schema, for on-prem and AWS cloud sites respectively. 

Use the **"Add new Template"** and create 2 new templates with following names. Similar to first template from **Actions** menu associate to respective sites. 

**AWS Template:**

- Template name: **temp-AWS-01**
- Template type: **ACI Multi-Cloud**
- Template Tenant: **Tenant-01**
- Site associated: **cAPIC**

Hit **Save** button to save the template. 

**on-prem Template:**

- Template name: **temp-on-prem-01**
- Template type: **ACI Multi-Cloud** 
- Template Tenant: **Tenant-01**
- Site associated: **on-prem**

Hit **Save** button to save the template. 

### 4. Application Profile and EPG Configuration for AWS 

ACI in Cloud doesn't use the concept of **Bridge Domain**, they don't have any representation in Cloud, that's why IP ranges were defined as part of the VRF configuration. 

**EPGs** can be now created and attached to VRF directly. 

Navigate to **temp-AWS-01** template using **View** dropdown menu. 

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/image100.png" width = 800>

Add **Application Profile** using **Add Application Profile** button. 

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/image101.png" width = 800>

- Display Name: **AppProf-AWS-01**
- Description: **Application Profile CL 2023 AWS**

Hit **Save**

Under created **Application Profile** add **EPG**

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/image102.png" width = 800>

- Display Name: **EPG-AWS-01**
- Description: **EPG AWS CL 2023** 
- EPG Type: **Application** 

Skip the **"On-Premises Properties"** and click on **"Cloud Properties"** and select **"Virtual Routing & Forwarding"** as **"VRF-01"**. 

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/image103.png" width = 500>

We also need to add **Selector**. Selectors are used in Public Cloud to assign Virtual Machines and Endpoints to correct EPG represented by a Security Group. Selector should be added Under **Site Specific** configuration for each EPG. 

Thare can be different type of selectores: 

- IP based 
- TAG based
- Region Based 
- Custom 

!!!Info 
    Selectors should be designed in a way that's Instances/Service are matched by only one selector - different configuration will result in Fault. 

In our case we will use **IP based** selectors. 

Under **Template Properties** swtich to **cAPIC** Site, click on **EPG-AWS-01** and hit **"Add Selector"** button under the EPG Cloud Properties. 

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/image104.png" width = 800>

Selector details: 

- Endpoint Selector Name: **AWS-sel**
- Expression: 
    
    - Type: **IP address**
    - Operator: **Equals**
    - Value: **10.0.0.0/23**

Hit checkbox sign to save expression and then **Ok** to finish. 

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/image105.png" width = 500>

Under **Template Properties** swtich back to **Template Properties** and hit **"Deploy to sites"** and **"Deploy"**. 

Once done confirmation will pop up on the screen. 

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/image106.png" width = 500>


### 5. Application Profile and EPG Configuration for on-prem APIC

In onprem ACI deployment, EPG is still assigned to Bridge-Domain. Before you create EPG, please create new Bridge Domain.

**on-prem Template:**

- Bridge Domain name: **BD-01**
- VRF Name: **VRF-01** 
- Gateway IP: **172.16.255.1/24**

Navigate to **temp-on-prem-01** template using **View** dropdown menu.
Scroll down to *Brdige Domain* section and click on Add Bridge Domain 

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/uc-1-bd-1.png" width = 500>

To add Gateway IP, scroll down right configuration section. You will find Gateway IP, click **Add Subnet**.

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/uc-1-bd-subnet.png" width = 500>

Add new Subnet, leave rest or properties as is.

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/uc-1-bd-subnet-1.png" width = 500>

When Bridge Domain BD-01 is ready, Continue with Application profile and EPG.

Navigate to **temp-on-prem-01** template using **View** dropdown menu. 

Add **Application Profile** using **Add Application Profile** button. 

- Display Name: **AppProf-01**
- Description: **Application Profile CL 2023**

Hit **Save**

Under created **Application Profile** add **EPG**

- Display Name: **EPG-on-prem-01**
- Description: **EPG CL 2023** 
- EPG Type: **Application** 

Configure the **"On-Premises Properties"** and select **"Bridge Domain"** as **"BD-01"**. 

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/image110.png" width = 800>


Hit **"Deploy to sites"** and **"Deploy"**. 

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/uc-1-onprem-deply-1.png" width = 800>

Once done confirmation will pop up on the screen. 

**At this point we have created Tenant, VRF, BD EPGs and selectors - let's now add security contracts.**


## Contract configuration 

### 1. Filter creation

In order to create Contract, we need to have a filter which defines what type of traffis is allowed or denied under specyfic contract subject. Filter can define what ports and protocols are matched by specyfic rule.

Open Nexus Dashboard Orchestrator GUI then go to **Application Management -> Schemas -> "Schema-T01"** -> open 

Under View select **"temp-stretch-01"** and **"Add filter"** under **Filter** section. 

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/image143.png" width = 800>

 - Display Name: **permit-any** 

Then click **"Add entry"** to define protocols and ports. 

 - Name: **permit-any** 
 
 Leave rest setting as default - this will allow for all protocols and ports, but we could limit traffic to very specific conditions. 

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/image144.png" width = 600>

Hit **Ok** to save it. 

### 2. Contract configuration

As contracts will be used to connect AWS EPG to on-prem EPG, contract should be created in stretched template so it's deployed in both sites. 

Under View select **"temp-stretch-01"** and **"Add contract"** under **Contract** section. 

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/image142.png" width = 800>

Create two(2) contracts with following details: 

- Contract **con-AWS-01-to-onprem-01** 

    - Display Name: **con-AWS-01-to-onprem-01** 
    - Scope: **VRF**
    - Apply both direction: **yes**
    - Add Filter: **permit-any**

Hit **Save** to finish contract configuration. 

- Contract **con-onprem-01-to-AWS-01** 

    - Display Name: **con-onprem-01-to-AWS-01** 
    - Scope: **VRF**
    - Apply both direction: **yes**
    - Add Filter: **permit-any**

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/image146.png" width = 800>

Hit **Save** to finish contract configuration. 

Hit **Deploy to sites** for contracts to be pushed. 

!!! Info 
    Even the checkbox for "Apply both direction" is enabled, we still need 2 contracts between Cloud EPGs. This configuration is coming from the fact that Consumed Contracts are adding Outbound/Outgoing rules and Provided Contracts are adding Inband/Incoming rules, so with one contract we would only allow for traffic to be initiated from consumer EPG. We could use the same contract in both direction (each EPG would then provide and consume the same contract), but it's not a best practice as may lead to unwanted traffic leaking and complicate troubleshooting. 

### 2. Contract assigment to EPGs 

Just creation of contract doesn't have any impact on the traffic. Contract needs to have at least one Provider EPG and one Consumer EPG to allow communication between them. 

Under View select **"temp-AWS-01"** click on the **EPG-AWS-01** and under EPG specific setting locate **Contract** section

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/image150.png" width = 800>

Hit **"Add Contract"** button and add the following

Contract 1: 

 - EPG: **EPG-AWS-01**
 - Contract: **con-AWS-01-to-onprem-01**
 - Type: **provider** 

Contract 2: 

 - EPG: **EPG-AWS-01**
 - Contract: **con-onprem-01-to-AWS-01**
 - Type: **consumer** 

 Hit **Deploy to sites** for contracts to be applied to EPGs. 

Under View select **"temp-on-prem-01"** click on the **EPG-on-prem-01** and under EPG specific setting locate **Contract** section



Hit **"Add Contract"** button and add the following

Contract 1: 

 - EPG: **EPG-on-prem-01**
 - Contract: **con-onprem-01-to-AWS-01**
 - Type: **provider** 

Contract 2: 

 - EPG: **EPG-on-prem-01**
 - Contract: **con-AWS-01-to-onprem-01**
 - Type: **consumer** 

Hit **Deploy to sites** for contracts to be applied to EPGs. 

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/image151.png" width = 800>

## EC2/VM creation and verification  

Next step is creation of AWS EC2 Instance for EPG associacion verification. 

### 1. AWS EC2 creation 

Login to AWS User tenant via https://console.aws.amazon.com and make sure that you have **London/eu-west-2** region selected.

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/image115.png" width = 300>

To be able to launch and login to VM we need to have SSH key-pair created. 

In the search bar type **"key pairs"** and select Key Pairs from Features list. 

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/image118.png" width = 600>

Hit **"Create key pair"** in top right corner and provide following settings: 

- Name: **AWS-key-pair**
- Key pair type: **RSA**
- Private key file format: **.pem**

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/image119.png" width = 600>

Hit "Create key pair" to finish. Once it's done key will be downloaded to your workstation. 

In the search bar type **"EC2"** and select EC2 from Service list. 

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/image116.png" width = 600>

On the EC2 Dashboard locate **"Launch instance"** and hit **"Launch instance"** option. 

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/image117.png" width = 600>

Instance details:

- Name: **VM-AWS-01**
- Application and OS Images: **Amazon Linux**
- Architecture: **64-bit** 
- Instance type: **t2.micro** 
- Key pair(login): **AWS-key-pair** 
- Network settings (hit Edit to change):

    - VPC: **context-[VRF-01]-addr[10.0.0.0/23]**
    - subnet: **subnet-[10.0.0.0/25]**
    - Auto-assign public IP: **Enable**
    - Firewall (security groups): **leave default**

- Other setting leave default 

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/image120.png" width = 800>

Review summary and hit **"Launch instance"** to create Virtual Machine

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/image121.png" width = 400>

It may take 3-5 minutes for instance to be ready - you may jump to next step and deploy instance in Azure, then come back here for verification. 


### 2. AWS EC2 verification 

Go back to EC2 -> Instances and locate your EC2 machine. Click on Instance-ID of you machine to open it. 

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/image122.png" width = 800>

 - Verify that VPC ID is correct 
 - Verify that subnet is correct 
 - Verify that instance has correct **Private IPv4 addressess"** 

 Note down IP address of EC2 instance, we will need it for verification. 

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/image123.png" width = 800>

 Go to Security tab settings of VM and check that **Security groups** for you instance is the one related to **EPG-AWS-01** 

 <img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/image124.png" width = 800>

!!! Info 

    This security group was created when EPG configuration was deployed towards AWS site. Instance interfaces were assigned to this EPG/Security Group based on the IP based Selector configured for this EPG. Cloud Network Controller is monitoring resources created in managed Tenants and it's automatically assigning Security groupes based on selectors. 
 
!!! Note

    After EC2 instance startup it may take some time for CNC to change security group to correct one. If no selector is matched, EC2 will be assigned to default Cloud Network Controller Secuirty Group will all traffic denied. 

