# OSPF MD5 Authentication, NTP, Syslog, SSh Configuration Guide
[Practical_File_All_In_One](Configure%20Routers_All.pkt)

![Image Alt Text](OSPF%20MD5%20authentication.png)

---

## Part 1: Configure OSPF MD5 Authentication
[Practical_File_1](OSPF_MD5_authentication.pkt)

### Step 1: Test Connectivity
Verify that all devices can ping each other's IP addresses.
### Step 2: Configure OSPF MD5 authentication for all the routers in area 0. 
Configure OSPF MD5 authentication for all the routers in area 0.
```bash

# On R1:
R1(config)# router ospf 1
R1(config-router)# area 0 authentication message-digest

# On R2:
R2(config)# router ospf 2
R2(config-router)# area 0 authentication message-digest

# On R3:
R3(config)# router ospf 3
R3(config-router)# area 0 authentication message-digest

```
### Step 3: Configure the MD5 key for all the routers in area 0
Configure an MD5 key on the serial
interfaces on R1, R2 and R3. Use the password MD5pa55 for key 1.

```bash

# On R1:
R1(config)# interface s0/0/0
R1(config-if)# ip ospf message-digest-key 1 md5 MD5pa55

# On R2:
R2(config)# interface s0/0/0
R2(config-if)# ip ospf message-digest-key 1 md5 MD5pa55
R2(config-if)# interface s0/0/1
R2(config-if)# ip ospf message-digest-key 1 md5 MD5pa55

# On R3:
R3(config)# interface s0/0/1
R3(config-if)# ip ospf message-digest-key 1 md5 MD5pa55
```

### Step 4: Verify configurations. 
```bash
# Use the following command to verify the OSPF MD5 authentication on the interfaces:

show ip ospf interface


# Ping tests again to verify end-to-end connectivity:
ping 192.168.1.1
```
---
## Part 2: C Configure NTP
[Practical_File_2](NTP.pkt)

### Step 1:  Enable NTP authentication on PC-A. 
A. On PC-A, click NTP under the Services tab to verify `NTP service is enabled`.

B. To configure NTP authentication, click Enable under Authentication. `Use key 1 and password NTPpa55`
for authentication. 

### Step 2: Configure R1, R2, and R3 as NTP clients

```bash
Router(config)# ntp server 192.168.1.5
Router(config)# ntp update-calendar

```
### Step 3: Configure R1, R2, and R3 as NTP clients

```bash
Router(config)# ntp authenticate
Router(config)# ntp trusted-key 1
Router(config)# ntp authentication-key 1 md5 NTPpa55
Router(config)# service timestamps log datetime msec
```
Exit global configuration and verify that the hardware clock was updated using the command `show clock` in Routers

---
## Part 3: Configure Routers to Log Messages to the Syslog Server
[Practical_File_3](Syslog_and_SSH.pkt)

```bash
Step 1: Configure the routers to identify the remote host (Syslog Server) that will receive
logging messages.
R1(config)# logging host 192.168.1.6
R2(config)# logging host 192.168.1.6
R3(config)# logging host 192.168.1.6
```
In Routers use the command `show logging` to verify logging has been enabled.

## Part 4: Configure R3 to Support SSH Connections 

```bash
Step 1: Configure the routers to identify the remote host (Syslog Server) that will receive
logging messages.
R3(config)# ip domain-name ccnasecurity.com
R3(config)# username SSHadmin privilege 15 secret ciscosshpa55
R3(config)# line vty 0 4
R3(config-line)# login local
R3(config-line)# transport input ssh
R3(config)# crypto key zeroize rsa
```
Note: If no keys exist, you might receive this message: % No Signature RSA Keys found in
configuration. 
```bash
R3(config)# crypto key generate rsa
How many bits in the modulus [512]: 1024
```
Verify the SSH configuration `show ip ssh`

```bash
R3(config)# ip ssh time-out 90
R3(config)# ip ssh authentication-retries 2
R3(config)# ip ssh version 2
```
Attempt to connect to R3 via Telnet from PC-C
```
PC> telnet 192.168.3.1
```
Connect to R3 using SSH on PC-C

```
PC> ssh –l SSHadmin 192.168.3.1
```
Connect to R3 using SSH on R2.
```
R2# ssh –v 2 –l SSHadmin 10.2.2.1
```