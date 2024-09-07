# Cisco Packet Tracer Lab: Configuring Static Routes

## Overview

This lab focuses on configuring static routes on Cisco routers to enable end-to-end connectivity between two PCs (PC1 and PC2) across different subnets. The lab scenario includes two routers (R1 and R2) interconnected by a serial link, each connected to a switch with a PC. The goal is to configure the IP addressing and routing on the devices to allow PC1 to ping PC2 successfully.

## Lab Objectives

1. **Device Configuration**: 
   - Assign hostnames and IP addresses as per the network diagram.
   - Configure the gateway on PCs.
   - Note: Switches do not require configuration in this lab.

2. **Static Route Configuration**: 
   - Configure static routes on routers R1 and R2 to enable connectivity between the PCs.

## Network Topology

- **Subnet 1 (192.168.1.0/24)**: 
  - **PC1** (IP: `192.168.1.1/24`, GW: `192.168.1.254`)
  - **R1** (G0/1: `192.168.1.254/24`)
  - **Switch 1**: 2960-24TT SW1 (No configuration required)

- **Subnet 2 (192.168.12.0/24)**: 
  - **R1** (G0/0: `192.168.12.1/24`)
  - **R2** (G0/0: `192.168.12.2/24`)

- **Subnet 3 (192.168.13.0/24)**: 
  - **R2** (G0/1: `192.168.13.1/24`)
  - **R3** (G0/0: `192.168.13.2/24`)

- **Subnet 4 (192.168.13.1/.24**
<img src="https://github.com/ro-drick/Configuring-Static-Routes/blob/main/configuring-static-routes.PNG"/>
## Solution

### 1. Device Configuration

#### a. **PC1 Configuration** (192.168.1.1/24)
1. Open PC1.
2. Go to the `Desktop` tab and open the `IP Configuration` tool.
3. Set the following:
   - **IP Address**: `192.168.1.1`
   - **Subnet Mask**: `255.255.255.0`
   - **Default Gateway**: `192.168.1.254`

#### b. **PC2 Configuration** (192.168.3.1/24)
1. Open PC2.
2. Go to the `Desktop` tab and open the `IP Configuration` tool.
3. Set the following:
   - **IP Address**: `192.168.3.1`
   - **Subnet Mask**: `255.255.255.0`
   - **Default Gateway**: `192.168.3.254`

#### c. **Router R1 Configuration**
1. Open Router R1.
2. Go to the `CLI` tab and enter the following commands:

```bash
    R1> enable
    R1# configure terminal
    R1(config)# hostname R1
    R1(config)# interface GigabitEthernet0/1
    R1(config-if)# ip address 192.168.1.254 255.255.255.0
    R1(config-if)# no shutdown
    R1(config-if)# exit
    R1(config)# interface GigabitEthernet0/0
    R1(config-if)# ip address 192.168.12.1 255.255.255.0
    R1(config-if)# no shutdown
    R1(config-if)# exit
    R1(config)# exit
```
#### d. Router R2 Configuration
1. Open Router R2.
2. Go to the CLI tab and enter the following commands:
    ```bash
    R2> enable
    R2# configure terminal
    R2(config)# hostname R2
    R2(config)# interface GigabitEthernet0/0
    R2(config-if)# ip address 192.168.12.2 255.255.255.0
    R2(config-if)# no shutdown
    R2(config-if)# exit
    R2(config)# interface GigabitEthernet0/1
    R2(config-if)# ip address 192.168.13.1 255.255.255.0
    R2(config-if)# no shutdown
    R2(config-if)# exit
    R2(config)# exit
    ```
#### e. Router R3 Configuration
1. Open Router R3.
2. Go to the CLI tab and enter the following commands:
```bash
R3> enable
R3# configure terminal
R3(config)# hostname R3
R3(config)# interface GigabitEthernet0/0
R3(config-if)# ip address 192.168.13.2 255.255.255.0
R3(config-if)# no shutdown
R3(config-if)# exit
R3(config)# interface GigabitEthernet0/1
R3(config-if)# ip address 192.168.3.254 255.255.255.0
R3(config-if)# no shutdown
R3(config-if)# exit
R3(config)# exit
```
### 2. Configuring Static Routes
Next, configure static routes on R1, R2, and R3 to ensure all subnets can communicate.

#### a. Static Route on R1
1. From the CLI of R1, add a route to the 192.168.3.0/24 network (PC2’s subnet):
```bash
R1> enable
R1# configure terminal
R1(config)# ip route 192.168.3.0 255.255.255.0 192.168.12.2
R1(config)# exit
R1# write memory
```
### b. Static Route on R2
From the CLI of R2, add routes for the networks on both sides (R1’s and R3’s):
```bash
R2> enable
R2# configure terminal
R2(config)# ip route 192.168.1.0 255.255.255.0 192.168.12.1
R2(config)# ip route 192.168.3.0 255.255.255.0 192.168.13.2
R2(config)# exit
R2# write memory
```
#### c. Static Route on R3
From the CLI of R3, add a route to the 192.168.1.0/24 network (PC1’s subnet):
```bash
R3> enable
R3# configure terminal
R3(config)# ip route 192.168.1.0 255.255.255.0 192.168.13.1
R3(config)# exit
R3# write memory
```
### 3. Testing Connectivity
- After configuring the static routes, you should be able to ping from PC1 to PC2 and vice versa.
#### a. Ping from PC1 to PC2
On PC1, open the Command Prompt from the Desktop tab.
Type the following command to test the connection:
```bash
ping 192.168.3.1
```
If the routing is configured correctly, the ping should succeed, showing that PC1 can reach PC2.
#### b. Ping from PC2 to PC1
On PC2, open the Command Prompt from the Desktop tab.
Type the following command to test the connection:
```bash
ping 192.168.1.1
```
Again, if the routing is correct, the ping should succeed.
<p><a href="">Here</a> is the unsolved practice lab</p>
## Conclusion

In this lab, we successfully demonstrated how to configure static routes on Cisco routers to enable communication between devices located on different subnets. By carefully assigning IP addresses to each device and setting up static routes on routers R1, R2, and R3, we ensured that PC1 and PC2 could communicate across multiple networks. This lab reinforced key networking concepts such as IP addressing, subnetting, and static routing, all of which are essential for managing inter-network communications in real-world network setups. Through testing, we verified that the static routes functioned as expected, allowing for successful end-to-end connectivity.

## Acknowledgements


Special thanks to **Jeremy's IT Lab** for providing valuable resources and tutorials that greatly contributed to the completion of this exercise. His in-depth explanations and practical demonstrations have been instrumental in enhancing my understanding of Cisco networking concepts and the effective use of Packet Tracer.

For more information and additional resources, visit [Jeremy's IT Lab](https://jeremysitlab.com/) and check out his YouTube for the full course, [Jeremy's IT Lab Free CCNA 200-301 | Complete Course](https://www.youtube.com/playlist?list=PLxbwE86jKRgMpuZuLBivzlM8s2Dk5lXBQ)
