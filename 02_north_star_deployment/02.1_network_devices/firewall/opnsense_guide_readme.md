# OPNsense Deployment Guide

This section documents the deployment of the firewall layer for the North Star Infrastructure. These firewalls protect the traffic from internal users to the internet, a DMZ which hosts Windows and Linux Servers, and also implements NAT translations and firewall policy.

## Prerequisites & Node Specification

Image: OPNsense-26.1-nano-amd64

Resources (Per Node):

- RAM: 2048 MB

- vCPUs: 2

- Qemu Binary: qemu-system-x86_64

## Physical Topology

For the full physical topology, see the [physical_topology](../../../01_design_documentation/topologies/physical_topology.md) page.

### Interface Mapping

For all the router interface mapping tables, see the [physical_interface_mapping](../../../01_design_documentation/physical_interface_mapping.md) page.

## Logical Topology

For the full logical topology, see the [logical_topology](../../../01_design_documentation/topologies/logical_topology.md) page.

### IP Address Mapping

For all the router IP addressing tables, see the [ip_addressing_and_vlans](../../../01_design_documentation/ip_addressing_and_vlans.md) page.

## Security & Compliance Hardening

For the security considerations that guide the North Star lab and applied to the routers, see the [network_design](../../../01_design_documentation/network_design.md) page.
