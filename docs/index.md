# Welcome to CiscoLive 2023 - Walk in Lab

Speakers:

*Karol Okraska*, CX Delivery Architect, Cisco Systems, Inc.

*Marcin Duma*, CX Delivery Architect, Cisco Systems, Inc.

## Cisco ACI support of Public Cloud infrastructure
Cisco Cloud Network Controller (formerly Cloud APIC) provides enterprises with networking tools necessary to accelerate their hybrid-cloud and/or multicloud journey.

Utilizing cloud-native constructs, the solution enables automation that accelerates infrastructure deployment and governance and simplifies management to easily connect workloads across multicloud environments. The Cisco Cloud Network Controller vision is to support enhanced observability, operations, and troubleshooting across the entire environment.

Cisco Cloud Network Controller enables:

●      Seamless connectivity for any workload at scale across any location

●      Operational simplicity and visibility across a vast multisite, multicloud

●      Data-center network

●      Easy L4-7 services integration

●      Consistent security and segmentation

●      Business continuity and disaster recovery

## Cisco Cloud Network Solution components: 

Cisco Cloud Network Controller is the main architectural component of this multicloud solution. It is the unified point of automation and management for the solution fabric including network and security policy, health monitoring, and optimizes performance and agility. The complete solution includes:

●      Cisco Cloud Network Controller ( called CNC later in labguide) (deployed in each Public Cloud which is to be managed) 

●      Cisco Nexus Dashboard (in our lab deployed on-prem) - Multicloud networking orchestration and policy management, disaster recovery, and high availability, as well as provisioning and health monitoring.

●      Cisco Catalyst® 8000V - deployed in Public Clouds, allowing for communication with other Clouds or on-premises datacenter. Responsible for traffic secuirty and end to end policy enforcement. 

## High Level Design of Lab scenario

The Lab excercise is about showing you practical use of stretching services between on-prem and Public Cloud leveraging Cisco Nexus Dashboard Orchestrator and CNC for it.
It gives you oportunity to check how easy can be maintaining your services in different places due to scalibility, costs, performance using Cisco Cloud products.
Figure below is showing Logical design you are going to deploy in the Lab. Infrastructure part is already deployed, you don't need to bother with NDO installation, CNC installation or day-1 configs. This lab is solely to familiraze you with deployment of logical topology from the figure.

!!! Info
	Lab is based on Cisco ACI simulator, thus it won't be possible to run Data-plane between on-prem and AWS. 
	If you are interested to deploy all infra in MultiCloud, please join our **Instructor Led Lab LTRCLD-2557**.


**Lab diagram:**

<img src="https://raw.githubusercontent.com/marcinduma/LABDCN-2542/master/images/image1a.png" width = 800>

As indicated in the diagram, this lab is using EMEA based regions in AWS as well as dCloud (onprem). 

POD runs on dedicated AWS infrastructure tenant which is already deployed and contains CNC and Cat8kv routers. Additionally each POD have dedicated AWS user tenant, its place where Policy model will be deployed.
Onprem, Lab infrastructure contains Cisco Nexus Dashboard with Nexus Dashboard Orchestrator, Cisco ACI infrastructure and CSR1kv to terminate IPSec tunnels.

Control plane infrastructure is already deployed for this lab. Use cases will be deployed in user tenant which needs to be onboarded in Cisco Network Controller and Nexus Dashboard Orchestrator.

**<p style="text-align: center;">Enjoy!</p>**