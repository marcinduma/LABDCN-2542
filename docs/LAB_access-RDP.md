# Lab access

## 1. Lab access general description

The lab has been built leveraging multiple environments as following:

- Amazon Web Services
- Private intrastructure on-prem

You will have access to Cisco dCloud Workstation, where you will be able to open WEB GUI to:
- Cisco Nexus Dashboard/NDO
- Cisco APIC onprem
- Cisco Nexus Controller - AWS


## 2. VPN connection to dCloud infrastructure

The entire lab for the session is built using Cisco dCloud environment.
Details of the session are provided in POD assigned to you. You will find there "Connect VPN" link which allow you connection to dCloud session.
When you decide to use BYOD, please connect to VPN using provided credentials in WIL assistant webpage.

### Access session with RDP

Once logged to the dCloud session, you will be able to RDP Windows workstation ready to use for you

Open RDP client and use credentials provided below:

IP Address:

	198.18.133.36

Username:
	
	dcloud\demouser

User password:
	
	C1sco12345

When you login to Remote Desktop, you will find installed Chrome as web browser, from where you get access to GUI resources.

## 1. Accessing Cisco Nexus Dashboard/NDO

Once you are connected to RDP workstation, please open Chrome and by using Bookmark bar select ***Nexus Dashboard***.

IP Address:

	198.18.134.200

Username:
	
	admin

User password:
	
	C1sco12345

## 2. Accessing Cisco APIC

Once you are connected to RDP workstation, please open Chrome and by using Bookmark bar select ***APIC***.

IP Address:

	198.18.133.200

Username:
	
	admin

User password:
	
	C1sco12345


## 3. Accessing Cisco Cloud Network Controller (cAPIC)

IP Address of your CNC will be provided during the lab in later stage. Each Pod has its own CNC, credentials to login are same for all of them:

Username:
	
	admin

User password:
	
	CiscoLive2@23!


## 4. Accessing AWS Console

Your access to AWS User tenant per POD

AWS console login link:

[https://console.aws.amazon.com](https://console.aws.amazon.com){target=_blank}

Azure portal login link: 

[https://portal.azure.com](https://portal.azure.com){target=_blank}

Information about account details as well as credentials are available in per POD sections listed in table below. Make sure to use corresponding POD ID from WiL dashboard.  

| Attendee Name           | POD ID       | POD access details       |
| -------------- | -------------- | -------------- |
| Student 1 | POD1 | [Lab Details POD1](pod1x.md)|
| Student 2 | POD2 | [Lab Details POD2](pod1x.md)|
| Student 3 | POD3 | [Lab Details POD3](pod1x.md)|
