# FHRP Redundancy Lab – HSRP, VRRP, GLBP

This lab demonstrates the use of **First Hop Redundancy Protocols (FHRPs)** in a simulated network environment using Cisco routers, switches, and host devices within **Cisco Modeling Labs (CML)**. It includes complete configurations and testing for:

* **HSRP (Hot Standby Router Protocol)**
* **VRRP (Virtual Router Redundancy Protocol)**
* **GLBP (Gateway Load Balancing Protocol)**

---

## 🧱 Lab Topology

The topology consists of:

* 3 Routers: `R1`, `R2`, `R3`
* 3 Layer 2 Switches: `SW1`, `SW2`, `SW3`
* 3 PCs: `PC1`, `PC2`, `PC3`

Each FHRP protocol is implemented on its own VLAN:

| Protocol | Routers Used | VLAN | Switch | PC  |
| -------- | ------------ | ---- | ------ | --- |
| HSRP     | R1 & R2      | 10   | SW1    | PC1 |
| VRRP     | R2 & R3      | 20   | SW2    | PC2 |
| GLBP     | R1 & R3      | 30   | SW3    | PC3 |

---

## 🔧 Technologies Implemented

### ✅ HSRP

* Implemented on VLAN 10
* R1 = Active, R2 = Standby
* VIP: `192.168.10.1`
* Hosts default to VIP as gateway
* Preemption configured manually on both routers

### ✅ VRRP

* Implemented on VLAN 20
* R2 = Master, R3 = Backup
* VIP: `192.168.20.1`
* Preemption is enabled by default

### ✅ GLBP

* Implemented on VLAN 30
* R1 = AVG, R1 & R3 = AVFs
* VIP: `192.168.30.1`
* Round-robin load balancing enabled

---

## 💻 End Device Configurations

Each PC was statically configured with:

| PC  | IP Address         | Gateway        |
| --- | ------------------ | -------------- |
| PC1 | `192.168.10.10/24` | `192.168.10.1` |
| PC2 | `192.168.20.10/24` | `192.168.20.1` |
| PC3 | `192.168.30.10/24` | `192.168.30.1` |

On Alpine Linux VMs, configuration was done via:

```bash
ip addr flush dev eth0
ip addr add <ip_address>/24 dev eth0
ip link set eth0 up
ip route add default via <gateway>
```

---

## 🧪 Testing and Validation

* Verified virtual IPs were reachable from each PC.
* Shut down Active/Master routers to test failover.
* Observed router role changes with:

  * `show standby` (HSRP)
  * `show vrrp` (VRRP)
  * `show glbp` (GLBP)
* Load balancing validated with MAC address tracking in GLBP.

---

## 🗂️ File Structure

```bash
.
├── hsrp_configs/
├── vrrp_configs/
├── glbp_configs/
├── topology_diagram.png
└── README.md
```

---

## 🔗 Related Links

* [HSRP on Cisco](https://www.cisco.com/en/US/tech/tk389/tk621/technologies_configuration_example09186a00800949c2.shtml)
* [VRRP RFC 5798](https://datatracker.ietf.org/doc/html/rfc5798)
* [GLBP Cisco Docs](https://www.cisco.com/c/en/us/td/docs/ios/12_2/ip/configuration/guide/1cglbp.html)

---

## 📘 Author

Created by [SergiNetworks](https://serginetworks.com/) as part of the CCNP ENCOR study lab series.
