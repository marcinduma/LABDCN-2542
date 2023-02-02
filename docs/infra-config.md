# Site Connectivity Configuration 

In Next Step will be verification of our Infrastructure connectivity.

Navigate to **Infrastructure** -> **Site Connectivity** -> **Configure** button

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/image33.png" width = 800>

On the **General Settings** Page, normally you will configure your global settings for entire MultiSite interconnection. Here you have BGP as well as OSPF polices, which will be deployed to every Site specified on the Left.

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/image34.png" width = 800>

General Settings section is divided to four different tabs. Each of tab is important to fill with correct data. IPN devices and External devices tabs contains information about IPN onprem as well as Cloud Cat8k devices respectively. Those informations are then used to creation of configuration to be deployed on onprem IPN devices and Cat8kv. At last tab - IPSec Tunnel Subnet pools, contains details about Site to Site connection types.

Go to tab **"IPSec Tunnel Subnet Pools"**, to specify your IPSec details for securing communication across public Internet.

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/image40.png" width = 800>

!!! Info
	You may notice that there is already one subnet 169.254.0.0/16 configured, which is used for Tunnel addresing between Cloud Routers and Cloud Load Balancers(Azure)/Transit Gateways(AWS). Those tunnles are used to forward traffic from Cloud instances to Cloud Routers and further to another sites.
	Second subnet 192.168.255.0/24 is used to address Tunnel interfaces between Sites in our case onpmrem CSR1kv router and Cat8kv routers in AWS.

In the left navigation bar, under the Sites bar, click on first site **cAPIC**

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/image35.png" width = 800>

You can verify site configuration on right side of the screen. Those information needs to be there.
    
    Settings:
    - ACI Multisite - checked 
    - Contract Based Routing - checked
	- Inter-Site connectivity - on-prem selected (BGP-EVPN)
	- Connection Type: Public

On-prem site have more information to define. All of them are deployed already to the site.

!!! Info
	You can explore rest of the Site connectivity sections by yourself or move to Logical configuration on the next page.

**In Next Section our First Tenant will be configured!**