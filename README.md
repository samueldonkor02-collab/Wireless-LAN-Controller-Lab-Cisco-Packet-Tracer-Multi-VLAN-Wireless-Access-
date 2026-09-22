# Wireless-LAN-Controller-Lab-Cisco-Packet-Tracer-Multi-VLAN-Wireless-Access-
Cisco Packet Tracer lab setting up a Wireless LAN Controller to manage three access points across separate VLANs, with router-on-a-stick routing and centralized DHCP for each wireless group.

# Wireless LAN Controller Lab (Cisco Packet Tracer, Multi VLAN Wireless Access)

`Cisco Packet Tracer` · `WLC 3504` · `Lightweight APs` · `Router on a Stick` · `Trunking` · `CompTIA Labs`

## Overview
This lab was hands on practice building a small enterprise wireless network in Cisco Packet Tracer. The goal was to take three separate groups of wireless users, each needing their own network segment, and get them all online through a single Wireless LAN Controller managing multiple lightweight access points. Instead of every group sharing one flat network, each group sits on its own VLAN with its own subnet, and a router on a stick handles the routing between them.

## Objective
Stand up a wireless network with three distinct SSIDs or groups, Azure Students, Network+ Students, and Mentors, each mapped to its own VLAN and subnet, all managed centrally through a WLC. Every wireless client needed to pull an address automatically from a shared DHCP server, and traffic between VLANs needed to route correctly through the main router.

## Environment
- Main Router: Cisco 2911, configured as a router on a stick with three subinterfaces
  - G0/0.10, 192.168.10.1, default gateway for VLAN 10
  - G0/0.20, 192.168.20.1, default gateway for VLAN 20
  - G0/0.30, 192.168.30.1, default gateway for VLAN 30, also the native VLAN
- DHCP Server: Server PT, sitting on its own 192.168.2.0/24 link straight into the router at 192.168.2.2, handing out addressing across VLANs 10, 20, and 30
- Core Switch: Cisco 2960 24TT, trunked down to the router and out to the WLC and all three access points
- Wireless LAN Controller: WLC 3504 at 192.168.30.200, sitting on the management native VLAN 30
- Access Points: three 3702i units, one labeled Azure Students, one labeled Network+ Students, and one labeled Mentors, each associated with its own VLAN
- Wireless Clients: a laptop on VLAN 10 (192.168.10.0/24), a laptop on VLAN 20 (192.168.20.0/24), a laptop on VLAN 30 (192.168.30.0/24), and a mentor laptop at 192.168.30.10 connecting wirelessly through the Mentors AP
- Simulated entirely in Cisco Packet Tracer

## Tools I Used

| Tool | What It Does | Why I Used It |
|------|---------------|----------------|
| Cisco Packet Tracer | Network simulation platform | Built and tested the whole topology, router, switch, WLC, APs, and wireless clients, in one place |
| WLC 3504 | Wireless LAN Controller | Centrally managed all three access points instead of configuring each AP by hand |
| 3702i Access Points | Lightweight APs | Provided wireless coverage for each group and stayed under WLC control rather than running standalone |
| Cisco 2911 Router | Router on a stick | Routed traffic between VLANs 10, 20, and 30 using subinterfaces on a single physical link |
| DHCP Server (Server PT) | DHCP service | Handed out addressing for all three VLANs from one central location |
| Catalyst 2960 24TT Switch | Layer 2 switching and trunking | Carried tagged VLAN traffic between the router, the WLC, and each access point |

## What I Did

### Laying Out the Topology
I built the network around a central switch, with the main router hanging off one side using subinterfaces for VLANs 10, 20, and 30, and the WLC and all three access points hanging off the other side over trunk links. A separate DHCP server connected directly into the router on its own subnet to keep addressing centralized without putting the server itself on a client VLAN.

### Configuring Router on a Stick
Since the router only had one physical link into the switch, I configured a subinterface for each VLAN, gave each one its own IP acting as the default gateway for that VLAN, and set VLAN 30 as the native VLAN for management traffic. This let one router interface handle routing for three separate broadcast domains.

### Trunking and VLAN Assignment
I set the link between the router and switch, and the links out to the WLC and access points, as trunks so VLAN tagged traffic could pass through cleanly. Each access point ended up tied to a specific VLAN so wireless clients associating with it would land on the correct subnet automatically.

### Setting Up the WLC and Access Points
I pointed the three 3702i access points at the WLC so they would run as lightweight APs under centralized management rather than each being configured individually. Each AP represented a different group, Azure Students, Network+ Students, and Mentors, and each one needed to hand clients off to the right VLAN.

### Verifying Wireless Client Addressing
I checked that each wireless laptop, whether on VLAN 10, VLAN 20, or VLAN 30, actually pulled an address from the DHCP server rather than needing anything static, and confirmed the mentor laptop connecting through the Mentors AP landed correctly on the 192.168.30.0/24 network.

## Skills I Picked Up
- Configuring router on a stick with subinterfaces so one physical interface can serve as the gateway for multiple VLANs.
- Understanding how a WLC and lightweight APs split responsibilities, with the controller handling configuration and management while the APs just forward wireless traffic.
- Seeing how VLAN assignment and trunking decide which subnet a wireless client ends up on, depending on which AP or SSID it connects through.
- Centralizing DHCP for multiple VLANs from a single server instead of standing up separate DHCP scopes at every segment.

## How This Applies in the Real World
Almost every organization with more than one department or user group on wireless needs some way to keep those groups logically separated, whether that is students versus staff, guests versus employees, or different departments entirely. Running a WLC with multiple lightweight APs mapped to different VLANs is the standard way to do that at scale, since it means new APs can be added or reconfigured from one place instead of touching every device individually. It also mirrors a common real world design where routing, switching, wireless, and DHCP each play a distinct role instead of one device trying to do everything.

## Where I'm Coming From
I'm making the jump into cybersecurity from a background in healthcare. It's a different field on paper, but a lot of the muscle memory carries over, following procedures carefully, protecting sensitive information, and staying calm and methodical when something isn't behaving the way it should. I'm currently studying for CompTIA Security+ and building labs like this one to get real hands on reps with core networking concepts, since a lot of security work assumes you already understand how the underlying network behaves.

## What I Want to Learn Next
- Digging deeper into actual WLC configuration, including WLAN profiles, security settings per SSID, and RF group configuration
- Practicing with overlapping VLANs or misconfigured trunk links to see how the network behaves when something is set up wrong
- Adding authentication to each SSID instead of relying on open association
- Capturing wireless client association and DHCP traffic in Simulation mode to see the process step by step instead of just confirming the end result

## Limitations and What I'd Do Differently in Production
- This was a simulated environment, so real concerns like RF interference, AP placement, and signal coverage weren't part of the exercise.
- No wireless security was configured on the SSIDs themselves. A production deployment would need WPA2 or WPA3 with proper authentication for each group.
- Only one WLC and one DHCP server were used. A production network serving multiple groups would typically want redundancy so a single device failure doesn't take down wireless or addressing for everyone.
- No access control was applied between VLANs beyond basic routing, so a production version would need ACLs to actually enforce separation between groups like students and mentors.

## References
- [Cisco Wireless LAN Controller Configuration Guide](https://www.cisco.com/c/en/us/support/wireless/wireless-lan-controller-software/products-installation-and-configuration-guides-list.html)
- [CompTIA Security+ (SY0-701) Exam Objectives](https://www.comptia.org/certifications/security)
- Cisco Packet Tracer, simulation environment used throughout
