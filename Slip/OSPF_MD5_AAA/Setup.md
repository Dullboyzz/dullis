# Cisco Packet Tracer Configuration for OSPF MD5 & AAA Authentication

## Step 1: Setup the Topology in Cisco Packet Tracer
1. Open **Cisco Packet Tracer**.
2. Drag and drop the following devices:
   - **1 Router (R1)**
   - **1 Switch**
   - **2 PCs (PC0 and PC1)**
3. Connect the devices using **copper straight-through cables**:
   - **PC0 → Switch (FastEthernet0)**
   - **PC1 → Switch (FastEthernet0)**
   - **Switch → Router (GigabitEthernet0/0)**
4. Verify the correct interfaces based on the topology.

## Step 2: Assign IP Addresses

### On Router (R1)
1. Click on **R1**, go to the **CLI** tab.
2. Enter the following commands:

```plaintext
enable
configure terminal
interface GigabitEthernet0/0
ip address 192.168.1.1 255.255.255.0
no shutdown
exit
```

### On PC0
1. Click on **PC0**, go to **Desktop > IP Configuration**.
2. Set the following:
   - **IP Address:** `192.168.1.2`
   - **Subnet Mask:** `255.255.255.0`
   - **Default Gateway:** `192.168.1.1`

### On PC1
1. Click on **PC1**, go to **Desktop > IP Configuration**.
2. Set the following:
   - **IP Address:** `192.168.1.3`
   - **Subnet Mask:** `255.255.255.0`
   - **Default Gateway:** `192.168.1.1`

## Step 3: Configure OSPF with MD5 Authentication on R1

### Enable OSPF
```plaintext
enable
configure terminal
router ospf 1
network 192.168.1.0 0.0.0.255 area 0
exit
```

### Enable MD5 Authentication
```plaintext
interface GigabitEthernet0/0
ip ospf authentication message-digest
ip ospf message-digest-key 1 md5 cisco123
exit
```

## Step 4: Configure Local User Authentication (AAA)

### Enable AAA on R1
```plaintext
enable
configure terminal
aaa new-model
aaa authentication login default local
username admin privilege 15 secret cisco123
exit
```

### Apply AAA Authentication to Console & VTY
```plaintext
line console 0
login authentication default
exit

line vty 0 4
login authentication default
exit
```

## Step 5: Verify Local AAA Authentication

### Test from R1 Console
1. Close the **CLI** and reopen it.
2. It should now ask for credentials:
   ```plaintext
   Username: admin
   Password: cisco123
   ```

### Test from PC0 & PC1
1. Open **PC0**.
2. Open **Command Prompt** and test connectivity:
   ```plaintext
   ping 192.168.1.1
   ```
3. Repeat the same for **PC1**.

## Final Step: Save Configuration


### Slip Practical Linke

https://itexamanswers.net/ccna-security-v2-0-labs-activities-instructions-answers-pt
