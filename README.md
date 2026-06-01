# Project North Star | Development Sandbox & Iteration

Active development repository in real-time with multiple document changes and technical design pivots as the North Star lab develops and improves over time.

It is a work-in-progress and is incomplete. For the final production release, see `north-star-prd.`

## Overview

North Star was designed to demonstrate skills in deploying a corporate network, optimizing the network for business needs, and securing the network to ensure business continuity.

This is achieved through automated infrastructure deployment, security best practices, endpoint monitoring, deployment and configuration of SIEM for centralized logging, backup and recovery, and vulnerability assessment. It is a cross-platform setup that includes Windows and Linux servers, clients, and network devices all virtualized within GNS3.

The repository has been created to simulate the level of details and documentation that I would implement for deploying infrastructure, tuning the routing networks, or monitoring an existing environment with the continual mindset of always improving. It is designed to read as if it was a real document for a production environment.  

Key objectives:

- Demonstrate detailed design and documentation skills
- Deploy infrastructure and servers to achieve business needs  
- Deploy endpoint monitoring, centralized logs, and real-time detection through dashboards
- Perform vulnerability testing and validate the effectiveness of deployed security measures
- Demonstrate a disaster recovery mindset and high availability design decision making

## Lab Devices & Applications

WAN

- GNS3 Cloud Node
- 3x VyOS Routers (MPLS)

Network & Edge (Both sites combined)

- 3x OPNsense Firewalls
- 2x Arista vEOS Spine Switches
- 5x Arista vEOS Leaf Switches
- 1x GNS3 Ethernet Switch

Compute Nodes

- 4x Proxmox Hosts
- 2x Windows 10 Client
- 2x Rocky Linux Client

Applications and Virtual Machines

- Veeam
- Splunk
- Nessus Tenable
- Wazuh
- Windows Server 2022
- Debian Linux

Infrastructure Monitoring

- Prometheus
- Grafana
- Ntopng

Automation

- Ansible Node

## Skills Demonstrated

:white_check_mark: Ansible deployed switches (vEOS)  
:white_check_mark: Firewall deployment and policy creation (OPNsense)  
:white_check_mark: SIEM deployment and endpoint security monitoring (Wazuh, Splunk Free)  
:white_check_mark: Backup services implemented, and recovery demonstrated (Veeam Community Edition)  
:white_check_mark: Vulnerability assessment on endpoints (Nessus Essentials)  
:white_check_mark: Conduct Attack simulations & analyse logs (Kali Linux)  
:white_check_mark: Server provisioning, configuration and management using Proxmox (Windows 2022, Debian)  
:white_check_mark: Documentation best practices and Git version control (GitHub, Codespaces/VSCode)  
:white_check_mark: Infrastructure Monitoring (Grafana, Prometheus, and ntopng)  

| Skills demonstrated | Description |
| ------------------- | ----------- |
| **Ansible Switch Deployment** | VLANs, OSPF, LACP, MLAG, and VARP |
| **Firewall Deployment and Policy Creation** | Deployment of two OPNsense firewalls in an HA stack with LAN, WAN, and DMZ zones |  
| **SIEM Deployment and Endpoint Monitoring** | Wazuh agents are installed on all Windows and Linux servers/clients and logs are forwarded to Splunk Free |
| **Backups** | Veeam Backup Server handles backups for Windows and Linux servers |
| **Vulnerability Scanning** | Nessus Essentials is used for periodic scans of targeted endpoints. There is a hard limit of five IP addresses that is enforced in Nessus Essentials |
| **Attack Simulation and Documentation** | Kali Linux is used for security testing scenarios such as targeting DMZ and internal hosts to test the effectiveness of the security systems and monitoring used in North Star. |
| **Windows & Debian Servers** | Windows Server 2022 evaluation used; includes DNS, DHCP, IIS and Active Directory. Debian (DMZ Apache Web server) and Rocky Linux is used for cross-platform examples |
| **Documentation and Git Version Control** | The setup of the project repository reflects Ansible directory best practices and applies a lean approach to folder structure where possible in general. This is stored on GitHub but developed using VSCode. Industry best practices are applied where appropriate such as Ansible Vault for secure password management |
| **Infrastructure Monitoring** | North Star utilizes various tools across three monitoring domains: Metrics, Network, and Security. Combined, the lab is able to answer the following questions: how much traffic is flowing, who is generating the traffic, and whether that traffic is suspicious |  

## Lab Scenarios

| Scenario | Scenario Action | Success Criteria |
| -------- | --------------- | ---------------- |
| **Simulated Attack** | Simulate attacks from Kali Linux either to a server or client | Blocked by the firewall, Wazuh detects, and the logs are sent to Splunk |
| **Endpoint Monitoring** | Conduct file changes or simulate failed login attempts | Detected by Wazuh and recorded in Splunk |
| **Routing / Resilience** | Simulate link failure to test the MLAG resiliency | Verify packets are not dropped due to the use of redundant links |
| **Backup & Recovery** | Delete a file on a client to simulate a lost critical file | Recover the file back to the client via Veeam restore |  
| **Vulnerability Management** | Nessus scans servers and clients | Successful scan and capture of vulnerabilities |

## Repository Structure

- [01_design_documentation](01_design_documentation) | High level network design documentation, IP addressing, and topologies.
- [02_north_star_deployment](02_north_star_deployment) | Ansible and device configuration files for deploying North Star.
- [03_scenario_testing](03_scenario_testing) | Lab scenario documentation

## How to Explore

1. Start with the Topology  
   Open the [system design documentation](01_design_documentation/1_1_system_design/system_design_document.md) which describes the high level decisions made for building North Star.  

   It also includes appendices such as the master [logical addressing table](01_design_documentation/1_2_detailed_design/appendices/appendix_b_logical_addressing.md) for all the IP address and VLANs used which are referenced in the system design documentation.

2. Review North Star Deployment Files  
   Open [north_star_deployment](02_north_star_deployment) to see detailed device configuration files used for all all devices deployed in GNS3 as well as the Ansible files used to deploy the Arista switches.  

   The most commonly used device in North Star is the [Arista switch](02_north_star_deployment/2_1_network_devices/switches/veos_switch_guide_readme.md) and the readme document contains the technical details on how they are used.

3. Read Lab Scenarios  
   Open [lab scenarios](03_scenario_testing/lab_scenarios.md) to read about the various testing scenarios conducted. The scenario pages will follow a format as follows:  
   - What was the scenario?
   - How was the scenario simulated?  
   - What was the result of the scenario?
   - Were any lab improvements needed as a result?

## Contact

- GitHub: <https://github.com/karletonz1>
- LinkedIn: <https://www.linkedin.com/in/karloc>


## Project Evolution | A Journey of Discovery

### Phase 1: In the beginning  

The initial choices made for the North Star lab revolved heavily around the resources available to deploy the lab in GNS3 with minimal cost. Some roadblocks were finding that some vendors required payment for using their official QCOW2 files needed for GNS3 appliances, and other hurdles were finding free alternatives but they did not have the full functionality needed to achieve the lab objectives. It was also important to be able to deploy this network by practicing automation, and Ansible was chosen for this purpose. A mix of manual configuration (bootstrap) was still required to get the initial networking up and running before fully configuring the network devices via Ansible Playbooks.

The phase 1 plan was to deploy the infrastructure network starting with the distribution and access layer switches, as well as the Ansible node (GNS3 Automation Network Node).

This phase had several challenges to overcome but it also had some valuable wins:

1. I initially chose to configure North Star using Open vSwitches (OVS), but I discovered that Ansible was unable to speak to the OVS via SSH. I tried numerous troubleshooting steps like installing necessary dependencies and modular automation content on the Ansible node and also trying community 'fixed' versions of OVS. I found that no version of OVS I tried had the SSH connection plugin installed.

2. A decision to pivot to API automation was made using Extreme EXOS switches instead. However, I found that these switches were defaulting to HTTP despite configuring them to only use HTTPS, and I could not get Ansible to speak to the switches. A final pivot to use Arista vEOS switches was made and these switches successfully communicated with my Ansible node.  

   Numerous physical and logical topology and IP addressing schema changes were made during this phase to reflect the various options tested to find a switch the worked. These updates also allowed for improvements to the structure of my GitHub repository to reflect best practices and to also consider and implement an active-active architecture through the use of MLAGs.  

3. The Ansible host did not have all the prerequisites to allow for automation via Rest-API which required updating via the internet.  

4. The various pivots allowed me to refine the development of creating standardized Ansible directory structures, inventory management, and also allowed me to practice verification and troubleshooting commands.  

   With the network switches confirmed, phase 2 focused on deploying the full switch configurations via automation and moving from a single-homed design to a dual-homed design using MLAG and LACP.

### Phase 2: Into the Automation Unknown  

This phase tested the concepts of Ansible automation on the Leaf switches before expanding the configuration to the Spines.

Challenges and Wins:

1. Since only a bootstrap configuration was done to the switches, single links were connected with no redundancy links due to MSTP running by default. This meant that as the playbook was run, the configuration was essentially cutting off Ansible's network access as it deployed the final configuration.  

   An OOBM switch was needed to solve this problem and to simulate enterprise environments where management and production networks are separated. Only one OOBM switch is used for simplicity within North Star, but it represented what would be done in environments like a data centre.  

   A connection from Ansible was attached to the dedicated OOBM switch, and the bootstrap configurations needed to be updated to move the management IP address to the management port. Links from the OOBM switch to all network devices were run to their respective management ports and management traffic was segmented by using a dedicated VRF management instance.  

   Proof of concept was achieved after the removal of the pre-existing connections via data ports, and successfully running the playbooks using the OOBM connections via a GNS3 Ethernet switch. Multiple tests using the new lean bootstrap configuration was done.

2. Numerous changes to physical and logical topologies were done to reflect this new addition to North Star.

3. The relative complexity of automating MLAG configuration meant there were several ways to write playbooks to achieve the desired configuration on the spines. This was invaluable experience testing the various ansible modules that would not only deploy the configuration, but also in the most efficient manner.

4. Blood, sweat, and tears were shed whilst trying to learn and create the playbooks needed to deploy the Arista leaf and spine switches. I encountered instances where unknown precedences were happening within the switches causing it to retain certain unwanted commands which broke the MLAG configuration. Ad-hoc solutions like adding 'no' commands within an Ansible task to remove the unwanted code, were needed to achieve 100% automated deployment up until the end of phase 2. The end of phase 2 indicates successful deployment of the full spine and leaf configurations, confirmed MLAG peer status, and port-channel active status from the leafs to their respective spines, MSTP is configured correctly and all 4 switches have received their assigned priorities, all through the use of Ansible.

### Phase 3: Beyond the Layer 2  

Now that the spines and leafs were operational at layer 2, it was time to focus on the layer 3 boundary between the spines and the routers. This required focusing on layer 3 addressing and configuration in the 'router domain' which runs Point-to-Point links between the routers-spines and routers-routers. It also signaled the start of the routing phase and the deployment of OSPF configuration on the spines and routers, which meant revisiting the Ansible playbooks and adding a new SVI and OSPF configuration section.

Challenges and Wins:

1. I've been able to learn a lot of nuances with Ansible and writing playbooks. Important ones that resonated with myself:
   - If your playbook fails at a certain point in your list, it won't proceed with any other tasks.
   - Case sensitivity and indentation is king. I've come to appreciate that YAML trades flexibility for readability. Every extra indentation or trailing spaces will bring up errors every time.
   - Using tools like `--syntax-check`, `--list-tasks`, `--check`, and even `Ansible-lint` are invaluable tools you can use when crafting Ansible playbooks.
   - When using context sub-menus such as `parents` in your tasks, ensure that you are thinking about where it fits in relation to the rest of the lines you are trying to run in your task.
   - I experienced ghosts within Ansible that cannot be explained. For instance, running a YAML file that includes running multiple playbooks that suddenly only does the first playbook, but changing the order forces it to run both playbooks again correctly.
   - When writing variables, ensure the values match what you have written in your host file.
2. As the repository directory grew as a result of adding more devices to North Star, it became clear that some overlap and redundant place holder directories needed to be reviewed and pruned so that it could become more efficient and less cluttered.

   The switch and router readme guides were also revamped. I originally included IP addressing tables and topology screenshots to these guides, but it quickly grew to become messy and cluttered. I pivoted to create central topology files and logical/physical mapping tables and referenced them as links within the guides. This served to compress the content for ease of viewing, and created a single source of truth documentation which made it easier to record North Star changes over time.

   The addition of the routers to the Ansible playbooks also meant a prune and reorder of the original file structure was required. I had created the same folders for both the routers and the switches as placeholders in the beginning, but as I understood more about Ansible's directory structures and where the files were being called, I was confident in co-locating files based on their functions within a common group_var and host_var directory as an example. This cut back on duplicates in the folder structure and created a cleaner repository going forward.

   Due to the large number of changes that needed to be made at this crucial juncture, I pivoted from configuring the repository solely on GitHub and onto VSCode on my PC. This made editing the repository structure a breeze once I understood the various nuances between the two, such as discovering the ability to single out commits before syncing them to GitHub, or combining multiple commits into one big commit.

   This transition was a learning curve, but a valuable one. It gave me a greater appreciation of the difference between working on a Linux workstation versus Windows when working with Ansible and Git.
3. Understanding how to operate VyOS routers was fairly simple for configuring basic router settings such as IP addresses and OSPF. As a right of passage, I came across the requirement to install the VyOS image onto the HDD of the appliance to allow for persistence and make `commit` and `save` commands work after reboot. This was essential in testing playbooks and deploying the routers from scratch as well as building on configuration beyond phase 3.

### Phase 4: On the Back of the Backbone Infrastructure  

The end of phase 3 marked the successful deployment of playbooks to configure all of the Leaf and Spine switches as well as both routers. Hardening configurations were also set which were appropriate for the point the lab was currently at. Authentication still required services and Domain Controllers to be provisioned, but this would be looked at during later phases.

Phase 4 took a different approach and the objective was to manually deploy the OPNsense firewalls as a baseline, test inter-VLAN routing, and validate link redundancy before applying Ansible automation before moving onto Phase 5.

To view what was tested and the results of the testing, see the [lab_scenarios](03_scenario_testing/lab_scenarios) page.

Challenges and Wins:

1. The initial bootstrap of the VyOS routers was relatively straightforward, but provisioning the firewalls presented a classic routing catch-22. I attempted to configure the bootstrapped firewalls from a Win10 client on VLAN 20. However, the firewalls were only aware of directly connected subnets and lacked a return route to the 10.0.0.0/16 network. A manual static route pointing to the connected router's IP was required to establish the initial HTTPS GUI connection

2. During initial testing, the OPNsense GUI was taking an unusually long time to load on the Win10 PC. Basic connectivity was established, so I initiated deep-dive packet captures to diagnose

   The packet analysis revealed that SYN-ACKs were not reaching the client, causing the PC to wait for timeouts before retransmitting SYN requests. By temporarily connecting the PC directly to the firewall, the issue disappeared, isolating the fault to a network issue. After ruling out MTU mismatches and TCP MSS clamping issues, I identified the root cause in the routing plane.

   The current network was relying on OSPF Equal-Cost Multi-Path (ECMP) routing at the router layer. While good for load-balancing stateless traffic, ECMP was causing asymmetric routing. The return traffic from the stateful firewall was taking a different path than the outbound traffic, causing the firewall to drop the packets.  

   The troubleshooting highlighted a limitation of traditional Layer 2 MLAG and Layer 3 ECMP designs when interacting with independent stateful perimeter devices, without additional tools to manage these limitations.  

   Some possible solutions were:
   1. Apply a local fix by forcing active/standby routing or heavily tuning OSPF path costs for all traffic.
   2. Enforce only a specific VLAN have access to the firewall and apply static routes to use specific paths for this network.  
   3. Treat this baseline failure as a catalyst to deploy a modern solution to the entire lab.

   Herein lay the ultimate Catch-22, the network design did intend to configure the firewalls as a synchronized High Availability cluster but because the ECMP routing was dropping my initial provisioning traffic, I couldn't reliably access the GUI to actually build the HA cluster in the first place.


### Phase 5: Back to the future

## Contact

- GitHub: <https://github.com/karletonz1>
- LinkedIn: <https://www.linkedin.com/in/karloc>
