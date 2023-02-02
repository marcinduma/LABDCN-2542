# Infrastructure configuration - MultiSite configuration

This lab section describes Nexus Dashboard functionalities. 

## Nexus Dashboard (ND) 

Cisco Nexus Dashboard is a single launch point to monitor and scale across different sites, whether it is Cisco Application Centric Infrastructure™ (Cisco ACI®) fabric controllers, the Cisco® Application Policy Infrastructure Controller (APIC), Cisco Nexus Dashboard Fabric Controller (NDFC), or a Cloud APIC running in a public cloud provider environment. Cisco NX-OS with Cisco Nexus Dashboard Fabric Controller (NDFC) – formerly Cisco Data Center Network Manager (Cisco DCNM) – is available as a service on the Cisco Nexus Dashboard. With third-party services integrated in Nexus Dashboard, NetOps can achieve command and control over global network fabrics, optimizing performance and attaining insights into data center and cloud operations. Using Cisco Nexus Dashboard, DevOps can improve the application deployment experience for multicloud applications Infrastructure-as-Code (IaC) integrations. Developers describe in code the networking components and resources needed to run an application in a data center or cloud.

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/nexus-dashboard-aag_0.png" width = 800>

### Nexus Dashboard overview

Go to your RDP session previously opened. Open Chrome web browser and from bookmarks select ***Nexus Dashboard***. Connect with login credentials:

Username:
	
	admin

User password:
	
	C1sco12345

After login to your Nexus Dashboard, you will land in "One view" section. It is usually a world map or table with your sites configured to be maintained from ND.

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/nd-oneview-1.png" width = 800>

You can zoom in/out to see it more in details. 

By changing the view to table by using *slider* marked in figure above, you will have such details on the screen. ***Launch*** link will open you APIC/CNC GUI in new web browser tab.

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/nd-oneview-2.png" width = 800>

By Clicking on **Admin Console** on Left navigation panel (marked red on figure above), you will be moved to System Overview Dashboard. Here you find information about your sites as well as Nexus Dashboard server status. You can check whether system is healthy, how many nodes in ND cluster you have, how many PODs/Deployments and Services are up and running. Simple graphical monitoring of entire system in one place.

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/nd-oneview-3.png" width = 800>

To explore more, you can click on Left Navigation panel to find out details information about Nexus Dashboard:

- Sites - sites configured to be maintain by ND cluster
- Services - Services installed on Nexus Dashboard - in our case Orchestrator (NDO)
- System Resources - details about ND cluster - PODs/Deployments/Services, etc
- Operations - Firmware management, backups, etc
- Infrastructure - ND infra
- Administrative - AAA configuration

In next section you will explore Nexus Dashboard Orchestrator - service which is running on ND cluster.