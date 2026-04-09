# VyOS Core Router Deployment Guide

This section documents the deployment and automation of the Core Routing layer for the North Star Infrastructure. These routers handle the L3 boundary between the distribution layer and the firewall perimeter.

## Prerequisites & Node Specification

Image: VyOS 1.5-rolling (vyos-2026.03.27-0030-rolling-generic-amd64)

Resources (Per Node):

- RAM: 2048 MB

- vCPUs: 4

- Qemu Binary: x86_64 (v8.0.4)

## Physical Topology

For the full physical topology, see the [physical_topology](../../../01_design_documentation/topologies/physical_topology.md) page.

### Interface Mapping

For all the router interface mapping tables, see the [physical_interface_mapping](../../../01_design_documentation/physical_interface_mapping.md) page.

## Logical Topology

For the full logical topology, see the [logical_topology](../../../01_design_documentation/topologies/logical_topology.md) page.

### IP Address Mapping

For all the router IP addressing tables, see the [ip_addressing_and_vlans](../../../01_design_documentation/ip_addressing_and_vlans.md) page.

## Bootstrap Configuration

Manual Bootstrap: Minimum configuration required via CLI to allow Ansible to reach the devices before pushing the remaining configuration via automation.

For the full bootstrap configuration required, see the [vyos_routers](../bootstrap_configs/vyos_routers.md) page.

## Ansible Orchestration

Once communication is established between Ansible and the routers, the following will be provisioned through automation:  
  :white_check_mark: Basic configuration such as system hostnames, compliance banners  
  :white_check_mark: Control plane hardening and RBAC (Individual admin accounts)  
  :white_check_mark: SNMP and Syslog configuration  
  :white_check_mark: NTP/DNS Settings for local synchronization  
  :white_check_mark: Routing (OSPF) Configuration

## Security & Compliance Hardening

For the security considerations that guide the North Star lab and applied to the routers, see the [network_design](../../../01_design_documentation/network_design.md) page.

