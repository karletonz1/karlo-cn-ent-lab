### Network Architecture Decisions

**Multi-Chassis Link Aggregation (MLAG):**  
MLAG allows Spine 01 and Spine 02 to appear as a single, logical Layer 2 switch to the downstream Extreme EXOS leaves. Unlike traditional Spanning Tree setups that block redundant links, MLAG provides an Active-Active Layer 2 topology. This allows the network to utilize 100% of the available bandwidth between the leaves and spines while maintaining sub-second failover.

**Virtual ARP (VARP):**  
VARP is used for First-Hop Redundancy across the fabric. Unlike VRRP or HSRP which rely on an Active/Standby model, VARP allows both Arista spines to route traffic simultaneously using the same Virtual MAC and Virtual IP address. This ensures that whichever spine receives a packet from a leaf switch can immediately route it, eliminating sub-optimal "tromboning" traffic across the MLAG peer link.

**Multiple Spanning Tree Protocol (MSTP):**  
While MLAG eliminates the need for STP to block uplinks, MSTP is deployed as a fail-safe mechanism against accidental Layer 2 loops (e.g., misconfigurations or physical loops on the EXOS access layer). The Arista Spines are configured as the Primary and Secondary MSTP Root Bridges to guarantee deterministic traffic flow. All VLANs are mapped to a single MST instance (MST0) to minimize CPU overhead on the switches.

## 3. High Availability Logic (VRRP)

Redundancy is managed via VRRP on top of the LACP Bond (bond0).

- VRID Strategy: The VRRP ID must match the VLAN ID (e.g., VLAN 20 = VRID 20).

- Preemption: Enabled on RTR-01 (Priority 200) to ensure it resumes Master status after a reboot.

- Hello Interval: 1 Second.


## 4. MTU & Performance Policy

Standard MTU (1500):

- Applied to all Management (OOB) interfaces.

- Applied to all WAN/Internet-facing interfaces.

- Applied to the Inter-Router LACP Backbone.

- Applied to Servers connections.  
ℹ️ In a production environment, there would be specific instances where an MTU higher than 1500 would be needed to accomodate jumbo frames. Specific storage devices have not been applied to this lab.
