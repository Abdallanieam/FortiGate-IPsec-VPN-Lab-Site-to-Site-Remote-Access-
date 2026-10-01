# FortiGate-IPsec-VPN-Lab-Site-to-Site-Remote-Access-
📌 Student Details

Student Name: Abdalla Mohamed Nieam

Platform: PNETLab

📖 Overview
This project demonstrates the design, configuration, and verification of a secure enterprise network architecture using FortiGate Next-Generation Firewalls (NGFW) inside a PNETLab environment.   
PDF
+ 1

The primary objective of this lab is to establish high-availability site-to-site connectivity using IPsec VPN between two geographically separated branch offices (Alexandria and Cairo), along with implementing secure Remote Access IPsec Dialup VPN for remote workforce clients using FortiClient.   
PDF
+ 1

🏗️ Network Topology
The lab topology emulates a multi-site enterprise network connected through a WAN/Internet simulation:   
PDF

🔵 Site A – Alexandria Branch (WE TELECOM)
Local Subnet: 172.16.30.0/24

   
PDF
+ 1

LAN Switch: Cisco Switch (SW1)   
PDF

Internal Hosts: Win10, Win13, Win14

   
PDF

Gateway Firewall: ALEX FortiGate (VM64-KVM)   
PDF
+ 1

LAN Interface (lan1 / port2): 172.16.30.1/24

   
PDF

WAN Interface (port1): 192.168.1.15/24

   
PDF

🟢 Site B – Cairo Branch (Orange)
Local Subnet: 50.50.50.0/24

   
PDF
+ 1

LAN Switch: Cisco Switch (SW2)   
PDF

Internal Hosts: Win7, Win8, Win9

   
PDF

Gateway Firewall: CAIRO FortiGate (VM64-KVM)   
PDF
+ 1

LAN Interface (lan1 / port2): 50.50.50.1/24

   
PDF

WAN Interface (port1): 192.168.1.17/24

   
PDF

🔴 WAN & Remote Access
WAN / Internet Cloud: Simulates public internet connectivity bridging both sites via 192.168.1.0/24.   
PDF

Remote Access Node: External PC running FortiClient accessing internal resources.   
PDF
+ 1

Dialup Interface (port3): Assigned client pool 12.1.1.1 – 12.1.1.2.   
PDF

🌐 Network & VPN Configurations
1. Dynamic IP Allocation (DHCP Server)
To automate network addressing for local clients, FortiGate DHCP Services are enabled on the LAN interfaces:   
PDF
+ 1

ALEX LAN Pool: 172.16.30.2 – 172.16.30.254 (Default Gateway: 172.16.30.1)   
PDF

CAIRO LAN Pool: 50.50.50.2 – 50.50.50.254 (Default Gateway: 50.50.50.1)   
PDF

2. Site-to-Site IPsec VPN Tunnel
Tunnel Interface: alex <---> cairo bound to WAN interface port1.   
PDF
+ 1

Routing: Static routes manually/automatically set to route target LAN subnets through the encrypted IPsec tunnel.   
PDF
+ 1

Security Policies: Bidirectional firewall rules allowing traffic between local LANs over the VPN interfaces.

3. Remote Access (Dialup) IPsec VPN
Interface: port3

   
PDF

Authentication Method: Pre-shared Key (IKEv1 / Aggressive Mode)   
PDF

Phase 1 Proposals: DES, MD5, DES-SHA1 | Diffie-Hellman Group 5   
PDF

Client IP Pool: 12.1.1.1 – 12.1.1.2/255.255.255.0

   
PDF

🧪 Verification & Testing
1. DHCP Lease Verification
Checked active client leases using FortiGate Dashboard (Network > DHCP).   
PDF
+ 1

Confirmed IP configuration on end devices using ipconfig (Win10 assigned 172.16.30.2).   
PDF
+ 1

2. Site-to-Site Connectivity Testing
Performed ICMP Ping tests between clients in Site A (172.16.30.0/24) and Site B (50.50.50.0/24), confirming 100% reachability across the IPsec tunnel.   
PDF

3. IPsec Tunnel Status & Session Table
Verified active tunnels in FortiOS IPsec Monitor showing active byte transfers (Incoming/Outgoing).   
PDF
+ 2

Executed CLI routing verification using:   
PDF

Bash
get router info routing-table all
4. Remote Access VPN Testing
Successfully established connection from the remote host using FortiClient VPN, confirming connected status and allocated IP address 12.1.1.1.   
PDF
+ 1

📊 Network Address Summary
Device / Interface	Role / Description	Subnet / IP Address
ALEX FortiGate (port1)

 
PDF

Site A WAN Interface	
192.168.1.15/24

 
PDF

ALEX FortiGate (port2)

 
PDF

Site A LAN Gateway	
172.16.30.1/24

 
PDF

CAIRO FortiGate (port1)

 
PDF

Site B WAN Interface	
192.168.1.17/24

 
PDF

CAIRO FortiGate (port2)

 
PDF

Site B LAN Gateway	
50.50.50.1/24

 
PDF

Site A DHCP Range

 
PDF

Client Address Pool	
172.16.30.2 – 172.16.30.254

 
PDF

Site B DHCP Range

 
PDF

Client Address Pool	
50.50.50.2 – 50.50.50.254

 
PDF

Remote Access Pool

 
PDF

FortiClient Dialup Pool	
12.1.1.1 – 12.1.1.2

 
PDF

📁 Repository Files
📄 Complete Report & Lab Screenshots
vpn,remote vpn pnetlab.pdf

   
PDF

Contains step-by-step GUI/CLI configurations, topologies, and test results.   
PDF

📦 PNETLab Topology & Configs

topology/pnetlab_vpn_topology.png

configs/alex_fortigate.conf

configs/cairo_fortigate.conf

🛠️ Technologies & Tools
PNETLab Virtualization Platform

   
PDF

FortiGate Next-Generation Firewall (FortiOS v7.0 KVM)

   
PDF

IPsec Site-to-Site VPN

   
PDF

IPsec Remote Access (Dialup) VPN

   
PDF

FortiClient VPN Agent

   
PDF

DHCP Services & Static Routing

   
PDF
+ 1

Cisco Switching

   
PDF

👨‍💻 Author
Abdalla Mohamed Nieam

Network & Security Engineer

GitHub: @Abdallanieam
