# OPNsense Bootstrap

## Purpose

This document serves as the initial configuration checklist for bootstrapping the OPNsense firewalls. It is the minimum configuration required to allow access to the firewall GUI through the North Star network. The deployment of the device will then take place solely on the firewall GUI.

## Prerequisites  

:white_check_mark: GNS3 topology is cabled according to the physical design.

:white_check_mark: All switches are powered on.

:white_check_mark: GNS3 console access is working.  

Node 1: Primary Firewall (fw01-primary)

> [!NOTE]
> Default credentials for a new vEOS switch is username: root, password: opnsense

## karlo-cn-fw-01 Bootstrap Config  

```text
Primary Firewall 

# Configure Interfaces

Select Option 1 and press Enter to select "Assign Interfaces".

Do you want to set up LAGGs now [y/n]?

    Type: n and press Enter.

 Do you want to set up VLANs now?

    Type: n and press Enter.

Enter the WAN interface name or 'a' for auto-detection:

    Type: vtnet0 and press Enter.

Enter the LAN interface name or 'a' for auto-detection:

    Type: vtnet2 and press Enter.

Enter the Optional 1 interface name or 'a' for auto-detection:

    Type: vtnet3 and press Enter.

Enter the Optional 2 interface name or 'a' for auto-detection:

    Type: vtnet1 and press Enter.

Enter the Optional 3 interface name or 'a' for auto-detection:

    Press Enter to leave blank and finish.

The interfaces will be assigned as follows:
WAN  -> vtnet0
LAN  -> vtnet2
OPT1 -> vtnet3
OPT2 -> vtnet1
Do you want to proceed? [y/n]

    Type: y and press Enter.

# IP Assignment & Securing Access

Type 2 and press Enter to select "Set interface IP addresses".

Configuring the LAN (vtnet2):

Enter the number of the interface you wish to configure:

    Type: 1 (for LAN) and press Enter.

Configure IPv4 address LAN interface via DHCP? [y/n]

    Type: n and press Enter.

Enter the new LAN IPv4 address:

    Type: 10.0.72.1 and press Enter.

Enter the new LAN IPv4 subnet bit count (1 to 32):

    Type: 30 and press Enter.

For a WAN, enter the new LAN IPv4 upstream gateway address. For a LAN, press <ENTER> for none:

    Press Enter.

Configure IPv6 address LAN interface via WAN tracking? [y/n]

    Type: n and press Enter.

Configure IPv6 address LAN interface via DHCP6? [y/n]

    Type: n and press Enter.

Enter the new LAN IPv6 address. Press <ENTER> for none:

    Press Enter.

Do you want to enable the DHCP server on LAN? [y/n]

    Type: n and press Enter.

Do you want to change the webGUI protocol from HTTPS to HTTP? [y/n]

    Type: n and press Enter.

Do you want to generate a new self-signed web GUI certificate? [y/n]

    Type: y and press Enter.

Restore web GUI access defaults? [y/N]

    Type: n and press Enter.

# Reset Root Password

Type 3 and press Enter to select "Reset Root Password".

Do you want to proceed? [y/N]:

    Type: y and press Enter.
```

## karlo-cn-fw-02 Bootstrap Config  

```text
Secondary Firewall 

# Configure Interfaces

Select Option 1 and press Enter to select "Assign Interfaces".

Do you want to set up LAGGs now [y/n]?

    Type: n and press Enter.

 Do you want to set up VLANs now?

    Type: n and press Enter.

Enter the WAN interface name or 'a' for auto-detection:

    Type: vtnet0 and press Enter.

Enter the LAN interface name or 'a' for auto-detection:

    Type: vtnet2 and press Enter.

Enter the Optional 1 interface name or 'a' for auto-detection:

    Type: vtnet3 and press Enter.

Enter the Optional 2 interface name or 'a' for auto-detection:

    Type: vtnet1 and press Enter.

Enter the Optional 3 interface name or 'a' for auto-detection:

    Press Enter to leave blank and finish.

The interfaces will be assigned as follows:
WAN  -> vtnet0
LAN  -> vtnet2
OPT1 -> vtnet3
OPT2 -> vtnet1
Do you want to proceed? [y/n]

    Type: y and press Enter.

# IP Assignment & Securing Access

Type 2 and press Enter to select "Set interface IP addresses".

Configuring the LAN (vtnet2):

Enter the number of the interface you wish to configure:

    Type: 1 (for LAN) and press Enter.

Configure IPv4 address LAN interface via DHCP? [y/n]

    Type: n and press Enter.

Enter the new LAN IPv4 address:

    Type: 10.0.72.9 and press Enter.

Enter the new LAN IPv4 subnet bit count (1 to 32):

    Type: 30 and press Enter.

For a WAN, enter the new LAN IPv4 upstream gateway address. For a LAN, press <ENTER> for none:

    Press Enter.

Configure IPv6 address LAN interface via WAN tracking? [y/n]

    Type: n and press Enter.

Configure IPv6 address LAN interface via DHCP6? [y/n]

    Type: n and press Enter.

Enter the new LAN IPv6 address. Press <ENTER> for none:

    Press Enter.

Do you want to enable the DHCP server on LAN? [y/n]

    Type: n and press Enter.

Do you want to change the webGUI protocol from HTTPS to HTTP? [y/n]

    Type: n and press Enter.

Do you want to generate a new self-signed web GUI certificate? [y/n]

    Type: y and press Enter.

Restore web GUI access defaults? [y/N]

    Type: n and press Enter.

# Reset Root Password

Type 3 and press Enter to select "Reset Root Password".

Do you want to proceed? [y/N]:

    Type: y and press Enter.
```

## karlo-cn-fw-01 and karlo-cn-fw-02 GUI Bootstrap Config  

```text
Initial GUI Configuration

Navigate to https://[10.0.72.1] and https://[10.0.72.9].

- Navigate to System > Settings > Administration. 

Set Hostnames: Navigate to System > Settings > General.

- Node 1: karlo-cn-fw-01
- Node 2: karlo-cn-fw-02
