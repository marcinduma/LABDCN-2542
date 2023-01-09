# Nexus Dashboard Orchestrator(NDO) 

**Cisco Nexus Dashboard Orchestrator (NDO)** provides consistent network and policy orchestration, scalability, and disaster recovery across multiple data centers through a single pane of glass while allowing the data center to go wherever the data is.

NDO allows you to interconnect separate Cisco® Application Centric Infrastructure (Cisco ACI®) sites, Cisco Cloud ACI sites, and Cisco Nexus Dashboard Fabric Controller (NDFC) sites, each managed by its own controller (APIC cluster, NDFC cluster, or Cloud APIC instances in a public cloud). The on-premises sites (ACI or NDFC in the future) can be extended to different public clouds for hybrid-cloud deployments while cloud-first installations can be extended to multi-cloud deployments without on-premises sites. 

The single-pane network interconnect policy management and the consistent network workload and segmentation policy provided by NDO allows monitoring the health of the interconnected fabrics, enforcement of segmentation and security policies, and performance of all tasks required to define tenant intersite policies in APIC, NDFC, and cAPIC sites. 

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/image23a.png" width = 800>

● Key features and benefits

-     Single pane of glass for administration and orchestration of multiple networking fabrics for both Cisco ACI and NDFC

-     Automation of the configuration and management of intersite network interconnects across an IP backbone for both Cisco ACI and NDFC

-     Consistent multitenant policy across multiple sites, which allows IP mobility, disaster recovery, and active/active use cases for data centers

-     Capability to map tenants, applications, and associated networks to specific availability domains within the Cisco Multi-Site architecture for both Cisco ACI and NDFC

-     Hybrid cloud and multi-cloud orchestration supporting on-premises Cisco ACI sites and public cloud sites (AWS and Azure)

-     Capability to have multi-cloud ACI deployments without on-premises sites

-     Scale out sites and leaf switches based on resource growth

●Use cases

There are several uses of Cisco Nexus Dashboard Orchestrator. Some of the main use cases include:

-     Large scale data center deployment

-     Data center interconnectivity

-     Cisco NDO multi-domain integrations

-     Hybrid cloud and multi-cloud

-     Service provider – 5G telco

**Let's now explore possibilites of Cisco Nexus Dashboard Orchestrator!**

### 1. NDO Service Introduction

**Nexus Dashboard Orchestrator** is already installed and enabled under **"Service Catalog"** inside **"Installed Service"** 

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/image27.png" width = 800>

Hit **"Open"** button in order to login 

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/image28.png" width = 400>

You will be now redirected to **Nexus Dashboard Orchestrator** Dashboard. 

### 2. NDO Overview

NDO Dashboard is very similar to ND Dashboard. You will see world map with sites located on them. You can change view to *Table* and get such summary on the screen.

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/ndo-overview-1.png" width = 800>

Similar to ND, Left navigation panel is divided to sections. You can find there:

- Sites - sites enablement under NDO. Sites are configured in ND itself but to be **Managed** by NDO must be explicitly configured in NDO Sites menu.

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/ndo-overview-2.png" width = 800>

- Application Management - under this section you find all components for Logical deployment of tenants. You will use this section while configuring Use-Cases in a moment.
- Fabric Management - configuration of Fabric and Access Polices to be apply at sites
- Operations - Backup/Restore/Updates etc
- Infrastructure - Site connectivity - definition of overlay-1 VXLAN evpn, OSPF, eBGP - described on next section
- Integration - Possibile integration with SDWAN or DNAC

On next page you will look on config definition for Inter-site communication.