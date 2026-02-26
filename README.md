# Linux Router Configuration Lab (Virtual Machine)

## Overview

This lab demonstrates how to configure a Linux system inside a virtual machine to function as a Layer 3 router.

The objective was to simulate two isolated networks and manually configure routing between them without using GUI tools or network simulators.

All configuration was completed entirely in the Linux terminal.

---

## Lab Environment

- Ubuntu Linux (Virtual Machine)
- QEMU Virtualization
- Network Namespaces
- Virtual Ethernet (veth) pairs
- iproute2 utilities
- sysctl for IP forwarding

---

## Network Topology

Two isolated networks were created:

Network A: 10.0.1.0/24  
Network B: 10.0.2.0/24  

Router Interfaces:
- 10.0.1.1/24
- 10.0.2.1/24

Simulated Hosts:
- PC1 → 10.0.1.2
- PC2 → 10.0.2.2

PC1 and PC2 were placed into separate network namespaces to simulate independent systems.

Traffic was routed between the two networks using manual Layer 3 configuration.

---

## Configuration Steps

### 1. Create Network Namespaces

sudo ip netns add PC1  
sudo ip netns add PC2  

---

### 2. Create Virtual Ethernet Pairs

sudo ip link add veth1 type veth peer name veth1-br  
sudo ip link add veth2 type veth peer name veth2-br  

---

### 3. Assign Interfaces to Namespaces

sudo ip link set veth1 netns PC1  
sudo ip link set veth2 netns PC2  

---

### 4. Assign IP Addresses

PC1 Configuration:

sudo ip netns exec PC1 ip addr add 10.0.1.2/24 dev veth1  
sudo ip netns exec PC1 ip link set veth1 up  
sudo ip netns exec PC1 ip link set lo up  

PC2 Configuration:

sudo ip netns exec PC2 ip addr add 10.0.2.2/24 dev veth2  
sudo ip netns exec PC2 ip link set veth2 up  
sudo ip netns exec PC2 ip link set lo up  

Router Interface Configuration:

sudo ip addr add 10.0.1.1/24 dev veth1-br  
sudo ip addr add 10.0.2.1/24 dev veth2-br  
sudo ip link set veth1-br up  
sudo ip link set veth2-br up  

---

### 5. Enable IP Forwarding

sudo /usr/sbin/sysctl -w net.ipv4.ip_forward=1  

Verification:

cat /proc/sys/net/ipv4/ip_forward  

Expected output:  
1

---

### 6. Configure Default Gateways

sudo ip netns exec PC1 ip route add default via 10.0.1.1  
sudo ip netns exec PC2 ip route add default via 10.0.2.1  

---

## Verification

Routing was verified by sending ICMP traffic from PC1 to PC2:

sudo ip netns exec PC1 ping 10.0.2.2  

Successful replies confirmed:

- Proper Layer 3 routing
- Correct subnet configuration
- Functional IP forwarding
- Working default gateways
- Inter-network communication between isolated namespaces

---

## Skills Demonstrated

- Linux networking administration
- Layer 3 routing fundamentals
- Subnetting and gateway configuration
- Network namespace isolation
- Packet forwarding configuration
- Virtual lab environment setup
- Troubleshooting PATH and system utilities


## Career Relevance

This lab reinforces foundational knowledge required for:

- CCNA-level routing concepts
- Network Engineer roles
- SOC Analyst traffic flow understanding
- Security Analyst infrastructure awareness
- Enterprise routing fundamentals


## Future Enhancements

- Implement firewall rules using iptables
- Capture traffic using tcpdump
- Simulate port scanning activity
- Expand into multi-router topology
- Introduce dynamic routing (FRRouting / OSPF)

---

Master’s in Cybersecurity in progress  
Network+ Certified  
Security+ (In Progress)  
CCNA Next  

Building real infrastructure experience through consistent hands-on labs.
