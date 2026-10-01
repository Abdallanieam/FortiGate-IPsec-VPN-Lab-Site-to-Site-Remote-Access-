# 🛡️ FortiGate IPsec Site-to-Site & Remote Access VPN Deployment

> **Enterprise-Grade Network Security & Remote Connectivity Architecture Implemented in PNETLab**

---

## 👨‍💻 Engineer Profile

* **Name:** Abdalla Mohamed Nieam[cite: 1]
* **Role:** Network & Security Engineer[cite: 1]
* **Platform:** PNETLab[cite: 1]
* **GitHub:** [@Abdallaniea](https://github.com/Abdallaniea)[cite: 1]

---

## 🎯 Executive Summary

This project demonstrates a comprehensive implementation of enterprise-grade network security and remote access solutions designed and validated using **FortiGate Next-Generation Firewalls (NGFW)** on **PNETLab**[cite: 1]. 

The primary objective is to establish a secure inter-branch infrastructure through an **IPsec Site-to-Site VPN** connecting two primary branch offices (**Alexandria Branch** and **Cairo Branch**), alongside a **Remote Access (Dialup) IPsec VPN** using **FortiClient** to allow remote users secure access to core internal subnets[cite: 1].

---

## 📐 Network Topology Architecture

```text
                               ┌─────────────────────────┐
                               │   WAN / Internet Cloud  │
                               │     (192.168.1.0/24)    │
                               └────────────┬────────────┘
                                            │
              ┌─────────────────────────────┴─────────────────────────────┐
              │                                                           │
     [port1: 192.168.1.15]                                       [port1: 192.168.1.17]
  ┌─────────────────────────┐                                 ┌─────────────────────────┐
  │ ALEX FortiGate (Site A) │ ◄══════ IPsec Tunnel ═════════► │CAIRO FortiGate (Site B) │
  └───────────┬─────────────┘                                 └───────────┬─────────────┘
     [port2: 172.16.30.1]                                        [port2: 50.50.50.1]
              │                                                           │
        (WE TELECOM)                                                (Orange Subnet)
       172.16.30.0/24                                             50.50.50.0/24
              │                                                           │
        ┌─────┴─────┐                                               ┌─────┴─────┐
        │ Cisco SW1 │                                               │ Cisco SW2 │
        └─┬───┬───┬─┘                                               └─┬───┬───┬─┘
          │   │   │                                                   │   │   │
        Win10 Win13 Win14                                           Win7 Win8 Win9
```[cite: 1]

---

## 🏢 Site Breakdown & Detailed Specifications

### 🔵 **Site A: Alexandria Branch (WE TELECOM)**
* **Firewall Node:** ALEX FortiGate (`FortiGate-VM64-KVM`, FortiOS v7.0.11)[cite: 1, 14]
* **WAN Interface (`port1`):** `192.168.1.15/24`[cite: 1, 15]
* **LAN Interface (`port2`):** `172.16.30.1/24` (Alias: `lan1`)[cite: 1, 15]
* **DHCP Scope:** `172.16.30.2` – `172.16.30.254`[cite: 1, 15]
* **Internal Workstations:** `Win10`, `Win13`, `Win14` connected via Cisco Switch `SW1`[cite: 1]

### 🟢 **Site B: Cairo Branch (Orange)**
* **Firewall Node:** CAIRO FortiGate (`FortiGate-VM64-KVM`, FortiOS v7.0.11)[cite: 1, 14]
* **WAN Interface (`port1`):** `192.168.1.17/24`[cite: 1, 14]
* **LAN Interface (`port2`):** `50.50.50.1/24` (Alias: `lan1`)[cite: 1, 18]
* **DHCP Scope:** `50.50.50.2` – `50.50.50.254`[cite: 1, 18, 19]
* **Internal Workstations:** `Win7`, `Win8`, `Win9` connected via Cisco Switch `SW2`[cite: 1]

### 🔴 **Remote Access (Dialup) VPN Configuration**
* **Inbound Gateway:** `port3` on ALEX FortiGate[cite: 1, 23]
* **Virtual Client IPv4 Pool:** `12.1.1.1` – `12.1.1.2/24`[cite: 1, 23]
* **Authentication Method:** Pre-shared Key (IKEv1 / Aggressive Mode)[cite: 1, 23]
* **Phase 1 Encryption & Proposal:** DES, MD5, DES-SHA1 | Diffie-Hellman Group 5[cite: 1, 23]
* **Client Software:** FortiClient VPN[cite: 1, 23]

---

## ⚙️ Core Technical Implementation Details

1. **Dynamic Host Configuration Protocol (DHCP):**
   * Configured directly on local LAN interfaces (`port2`) on both FortiGate firewalls[cite: 1, 15, 18].
   * Automatically provisions Gateway, Subnet Mask, and DNS parameters to LAN workstations (`Win10`, `Win7`, etc.)[cite: 1, 15, 16, 18].

2. **IPsec Site-to-Site VPN Tunneling:**
   * Established Phase 1 and Phase 2 IPsec tunnel interfaces (`alex` $\leftrightarrow$ `cairo`) bound to WAN `port1`[cite: 1, 19, 20].
   * Built static routing entries to route destination subnet traffic (`50.50.50.0/24` and `172.16.30.0/24`) through the encrypted VPN interfaces[cite: 1, 19, 20].

3. **Firewall Policies & Access Control:**
   * Defined strict inbound/outbound Firewall Rules allowing cross-site communication while filtering unwanted traffic[cite: 1].
   * Enabled Blackhole Routing to prevent routing loops for unreachable VPN subnets[cite: 20].

---

## 🧪 Testing, Audit & Verification Results

* **DHCP Leasing Verification:** Verified active client IP lease bindings under `Dashboard -> Network -> DHCP`[cite: 1, 17, 19].
* **ICMP Ping & End-to-End Connectivity:** Confirmed 100% reachability across branches (`172.16.30.x` $\leftrightarrow$ `50.50.50.x`) using Command Prompt tests[cite: 1, 21].
* **IPsec Session & Data Audit:** Checked tunnel status, active byte counters (Tx/Rx), and CLI routing tables using[cite: 1, 22, 23]:
  ```bash
  get router info routing-table all
  ```[cite: 1, 23]
* **Remote Access Dialup Validation:** Successfully authenticated remote clients via **FortiClient**, receiving IP `12.1.1.1` from the defined Virtual Pool[cite: 1, 23].

---

## 📊 Configuration Reference Table

| Appliance / Interface | Role | Configured IP / Address Range |
| :--- | :--- | :--- |
| **ALEX FortiGate (`port1`)** | Site A WAN Interface | `192.168.1.15/24`[cite: 1, 15] |
| **ALEX FortiGate (`port2`)** | Site A LAN Gateway | `172.16.30.1/24`[cite: 1, 15] |
| **CAIRO FortiGate (`port1`)** | Site B WAN Interface | `192.168.1.17/24`[cite: 1, 14, 18] |
| **CAIRO FortiGate (`port2`)** | Site B LAN Gateway | `50.50.50.1/24`[cite: 1, 18] |
| **Site A DHCP Scope** | Local Clients Subnet | `172.16.30.2 – 172.16.30.254`[cite: 1, 15] |
| **Site B DHCP Scope** | Local Clients Subnet | `50.50.50.2 – 50.50.50.254`[cite: 1, 18, 19] |
| **Remote Access Pool** | Dialup VPN Clients | `12.1.1.1 – 12.1.1.2`[cite: 1, 23] |

---

## 🛠️ Stack & Technologies

* **Simulation Engine:** PNETLab[cite: 1]
* **Firewall OS:** FortiGate NGFW (FortiOS v7.0.11)[cite: 1, 14]
* **Security Protocols:** IPsec, IKEv1, DES/MD5, DH Group 5[cite: 1, 23]
* **Client Access:** FortiClient VPN[cite: 1, 23]
* **Switching & Routing:** Cisco IOS, Static Routing, DHCP Server[cite: 1]

---

👨‍‍💻 **Author:** [Abdalla Mohamed Nieam](https://github.com/Abdallaniea) — *Network & Security Engineer*[cite: 1]
