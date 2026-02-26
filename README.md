# Linux Router Configuration Lab (Virtual Machine)

## Overview

This lab demonstrates how to configure a Linux system inside a virtual machine to function as a Layer 3 router.

The objective was to simulate two isolated networks and manually configure routing between them without using GUI tools or network simulators.

All configuration was completed using the Linux terminal.

---

## Lab Environment

- Ubuntu Linux (Virtual Machine)
- QEMU virtualization
- Network namespaces
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

Simulated hosts:
- PC1 → 10.0.1.2
- PC2 → 10.0.2.2

PC1 and PC2 were placed into separate network namespaces to simulate independent systems.

---

## Configuration Steps

### 1. Create Network Namespaces

```bash
sudo ip netns add PC1
sudo ip netns add PC2
