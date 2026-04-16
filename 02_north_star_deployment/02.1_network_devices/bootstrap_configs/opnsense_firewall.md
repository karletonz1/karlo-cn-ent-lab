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

    Type: 10.0.70.24 and press Enter.

Enter the new LAN IPv4 subnet bit count (1 to 32):

    Type: 31 and press Enter.

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

    Type: 10.0.70.30 and press Enter.

Enter the new LAN IPv4 subnet bit count (1 to 32):

    Type: 31 and press Enter.

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

Navigate to https://[10.0.70.24] and https://[10.0.70.30].

- Navigate to System > Settings > Administration. 

Set Hostnames: Navigate to System > Settings > General.

- Node 1: karlo-cn-fw-01
- Node 2: karlo-cn-fw-02
```

## Phase 2 Linkage

The VyOS Border Leafs (karlo-cn-prd-bl01 & bl02) have been configured with /31 transit links facing the firewalls (10.0.70.24/31 through 10.0.70.30/31).

Complete the following via the GUI:

### Step 1: Install the FRR/BGP Plugin in OPNsense

**Navigate to the Plugins Menu:**  

Go to System -> Firmware -> Plugins.  

**Install FRR:**  

- In the search bar, type os-frr.  

- Click the + (Install) icon next to the plugin.  

- Wait for the installation to complete, then refresh your browser.  

**Enable the Routing Daemon:**  

- You will now see a new Routing menu in the left-hand navigation pane.  

- Go to Routing -> General.  

- Check the box for Enable to turn on the FRR daemon.  

- Click Save.  

### Step 2: Establish Peer to the Border Leafs

**Configure the BGP Base Settings:**  

- Navigate to Routing -> BGP -> General.  

- Enable BGP: Check the box.  

- BGP AS Number: Enter 65250 (or your chosen Edge AS).  

- Router ID: Enter 10.0.71.150 (from your IP Schema).  

- Leave the Network field blank for now (we will handle the default route in Step 3).  

- Click Save.  

**Add Neighbor 1 (Border Leaf 01):**  

- Navigate to Routing -> BGP -> Neighbors.

- Click the + button to add a new peer.

- Enabled: Check the box.

- Peer IP: 10.0.70.25 (BL01's interface IP).

- Remote AS: 65101.

- Update Source: Select vtnet2  

- Click Save.

**Add Neighbor 2 (Border Leaf 02):**  

- Still under Neighbors, click the + button again.  

- Enabled: Check the box.  

- Peer IP: 10.0.70.27 (BL02's interface IP).  

- Remote AS: 65102.  

- Update Source: Select vtnet3  

- Click Save.  

### Step 3: Default Route (0.0.0.0/0)

This forces the firewall to advertise a 0.0.0.0/0 route to the Border Leafs, making the firewall the default exit node for the fabric.

- Navigate back to Routing -> BGP -> Neighbors.

- Edit the Neighbor 1 (10.0.70.25) configuration.

- Scroll down to the Advanced section.

- Locate the checkbox for Default originate

- Check the box and click Save.

- Repeat this exact process for Neighbor 2 (10.0.70.27).

- Finally, navigate to Routing -> Diagnostics -> General to restart the FRR service and apply the changes.

### How to configure the Loopback in OPNsense  

To assign that 10.0.71.150/32 IP so BGP can use it as a Router ID, you will do this in the OPNsense GUI:  

**Create the Interface:**

- Go to Interfaces -> Other Types -> Loopback.  

- Click the + (Add) button.  

- Give it a description and click Save.

**Assign the Interface:**  

- Go to Interfaces -> Assignments.

-At the bottom of the list, select your new lo1 interface from the dropdown next to "New interface" and click + Add.

**Configure the IP:**  

- Click on the newly assigned interface in the left-hand menu

- Enable the interface.

- Set the IPv4 Configuration Type to Static IPv4.

- Enter your IP: 10.0.71.150 and set the subnet mask to /32.

- Click Save and Apply Changes.

### Step 4: Physical Interface & MTU Configuration

Assign IPs to the physical interfaces if this has not been done as part of the initial configuration.  

**Assign the Interfaces:**  

- Go to Interfaces -> Assignments.

- Assign all outstanding interfaces.

**Configure IP and MTU:**  

- Click on each newly assigned interface in the left-hand menu.

- Enable the interface.

- Set IPv4 Configuration Type to Static IPv4.

- Enter the IPs from the Phase 2 schema.

- Ensure the MTU field is left blank or explicitly set to 1500.

- Click Save and Apply Changes.

### Validation

To confirm the firewall is successfully participating in the fabric underlay:

- Go to Routing -> Diagnostics -> BGP. You should see both 10.0.70.25 and 10.0.70.27 listed with a state of Established or a numerical prefix count.

- In the VyOS Border Leafs: Run show ip bgp summary. You should see the .24 and .26 IPs from FW01 listed as established peers, and checking the routing table (show ip route) should reveal the 0.0.0.0/0 route learned via BGP.

>[!NOTE]
> Detailed configuration screenshots and HA setup to follow in Phase 4
