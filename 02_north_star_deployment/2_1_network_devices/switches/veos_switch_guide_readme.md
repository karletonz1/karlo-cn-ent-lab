# Arista vEOS Switch Deployment Guide

This section documents the deployment and automation of the switch environment using Arista vEOS Switches for the North Star Infrastructure. These switches are used to simulate the aggregation and access layer both respectively labelled spine and leaf. MLAG will be deployed to ensure at least 2 links are connected between all switches, and Point-to-Point links with SVI will be configured for the links connecting the spines to the VyOS Routers.  

The verification section includes screenshots that show successful deployment of the switches via Ansible.

## Prerequisites & Node Specification

Image: Arista vEOS 4.35.3F

Resources (Per Node):

- RAM: 2048 MB
- vCPUs: 1
- Qemu Binary: x86_64 (v8.0.4)

## Physical Topology

For the full physical data and management plane topologies, see the [physical topology](../../../01_design_documentation/1_1_system_design/appendices/appendix_a_physical_topology.md) page.

> [!NOTE]  
> In order to provision the devices using Ansible with only a bootstrap configuration, an out-of-band-management (OOBM) network was needed to replicate what would be done in a production environment with a separate OOBM network. This lab simulates this by using a separate switch connected to the network devices via a dedicated VRF management network on vlan 10.  

### Interface Mapping

For the interface mapping tables for all switches, see the [physical interface mapping](../../../01_design_documentation/1_2_detailed_design/appendices/appendix_c_port_mapping.md) page.

## Logical Topology

For the full logical topology, see the [logical topology](../../../01_design_documentation/1_1_system_design/appendices/appendix_b_logical_topology.md) page.

### IP Address Mapping

For the spine and leaf IP addressing tables, see the [logical IP addressing](../../../01_design_documentation/1_2_detailed_design/appendices/appendix_b_logical_addressing.md) page.

## Bootstrap Configuration

Manual Bootstrap: Minimum configuration required via CLI to allow Ansible to reach the devices before pushing the remaining configuration via automation.

For the full bootstrap configuration required, see the [arista_switches](../bootstrap_configs/arista_switches.md) page.

## High Availability and Redundancy

  Design:
  This lab uses MLAG at layer 2 and VARP at layer 3 in order to eliminate single points of failure. By using MLAG at Layer 2 and VARP at Layer 3, the design achieves an active-active network state. This provides a resilient Virtual IP gateway for all end-host subnets, which allows for fail-over to the redundant links seamlessly.

## Ansible Network Automation

  :white_check_mark: Basic configuration such as system hostname and compliance banners  
  :white_check_mark: Control plane hardening and RBAC  
  :white_check_mark: SNMP and Syslog configuration  
  :white_check_mark: NTP/DNS Settings  
  :white_check_mark: MLAG and LACP configuration  
  :white_check_mark: VLAN and Trunking configuration  
  :white_check_mark: MSTP configuration  

## Security & Compliance Hardening

For the security considerations that guide the North Star lab and applied to the switches, see the [system design document](../../../01_design_documentation/1_2_detailed_design/detailed_design_document.md) page.

## Verification

### Ansible 'Deploy North Star' Playbook for Phase 1a  

To view the screenshots that show successful deployment of Phase 1a Ansible playbook, see the [phase 1a verification](../../../01_design_documentation/1_3_phase_verification/phase_1a_verification.md) page.  

### Ansible 'Deploy North Star' Playbook for Phase 1b

To view the screenshots that show successful deployment of Phase 1b Ansible playbook, see the [phase 1b verification](../../../01_design_documentation/1_3_phase_verification/phase_1b_verification.md) page.
