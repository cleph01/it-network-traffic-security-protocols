<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Network Traffic Examination"/>
</p>

# Demonstrating Network Fundamentals and Traffic Analysis in Azure

## Overview

This lab showcases fundamental networking concepts within Microsoft Azure by examining network traffic to and from Azure Virtual Machines (VMs) residing within the same virtual network. By utilizing Wireshark, a powerful protocol analyzer, and interacting with Azure's Network Security Groups (NSGs), we will observe and analyze various network protocols in action. This hands-on exercise demonstrates practical experience in network communication, security controls, and traffic analysis, relevant skills for IT support and help desk roles.

## Environments and Technologies

- **Microsoft Azure:**
  - Virtual Machines (Compute Service)
  - Virtual Networks
  - Network Security Groups (NSGs)

- **Operating Systems:**
  - Windows 10 (Version 21H2)
  - Ubuntu Server 20.04
    
- **Protocols:** ICMP, SSH, DHCP, DNS, RDP

- **Tools:**
  - Remote Desktop Connection
  - Command Prompt
  - PowerShell
  - Wireshark (Protocol Analyzer)

---

## Part 1: Azure Virtual Machine Environment

This section outlines the existing Azure virtual machine environment used for this lab.

1.  **Windows Virtual Machine (Existing):**
  * An existing Windows 10 virtual machine was utilized.
  * This VM is located within a pre-existing Azure Virtual Network named "Active-Directory-Vnet".
  * A Network Security Group (NSG) is already configured for this VM, allowing inbound Remote Desktop Protocol (RDP) traffic on port 3389.

  <p align="center">
    <img width="1138" alt="Screenshot of Existing Windows VM Configuration in Azure Portal" src="https://github.com/user-attachments/assets/e79746cb-d762-4719-874d-0cccd0e76ec5" />
  </p>
  <p align="center">
    <em>Screenshot of the existing Windows 10 Virtual Machine configuration details within the Azure portal.</em>
  </p>

  <p align="center">
    <img width="1277" alt="Screenshot of Existing Windows VM Network Settings in Azure Portal" src="https://github.com/user-attachments/assets/6742a427-0a2d-42f4-869b-e8ccc2d229dc" />
  </p>

---

2.  **Linux (Ubuntu) Virtual Machine:**
  * A new Ubuntu Server 20.04 virtual machine was created for this lab.
  * During creation, this VM was explicitly placed within the **same** virtual network ("Active-Directory-Vnet") and subnet as the existing Windows 10 VM.
  * The Network Security Group (NSG) associated with this Ubuntu VM was configured to allow inbound Secure Shell (SSH) connections on port 22.

  <p align="center">
    <img width="932" alt="Screenshot of Ubuntu VM Creation - Networking Tab" src="https://github.com/user-attachments/assets/6e072d9b-0c7e-475c-af6b-bd5700256902" />
  </p>
  <p align="center">
    <em>Screenshot showing the network configuration of the newly created Ubuntu VM, confirming its placement within the "Active-Directory-Vnet".</em>
  </p>

  <p align="center">
    <img width="1135" alt="Screenshot of Both VMs in the Azure Portal Resource Group" src="https://github.com/user-attachments/assets/35cc608a-9bc6-4669-b27d-f2455315a5fb" />
  </p>
  
---

## Part 2: Observing Internet Control Message Protocol (ICMP) Traffic

This section details the process of observing ICMP traffic using Wireshark.

1.  **Connect to the Windows 10 VM and Install Wireshark:**
  * A Remote Desktop Connection was established to the Windows 10 virtual machine.
  * Wireshark was downloaded and installed on the Windows 10 VM from the official website ([https://www.wireshark.org/](https://www.wireshark.org/)).

  <p align="center">
    <img width="1130" alt="Screenshot of Wireshark Installation on Windows 10 VM" src="https://github.com/user-attachments/assets/8adb6914-1ff1-40dd-a02a-c0194a5202a6" />
  </p>
  <p align="center">
    <em>Screenshot showing the Wireshark installation process on the Windows 10 virtual machine.</em>
  </p>

---

2.  **Start Wireshark Capture:**
  * Wireshark was launched.
  * The appropriate network interface (the primary Ethernet adapter associated with Azure) was selected for packet capture.
  * The capture was initiated by clicking the "Start capturing packets" icon.

  <p align="center">
    <img width="1125" alt="Screenshot of Wireshark Interface with Capture Options" src="https://github.com/user-attachments/assets/0ca42d0a-4950-4613-b43a-f81ac1185b86" />
  </p>
  <p align="center">
    <em>Screenshot of the Wireshark interface displaying the available network interfaces before starting the capture.</em>
  </p>

  <p align="center">
    <img width="412" alt="Screenshot of Wireshark Start Capture Icon" src="https://github.com/user-attachments/assets/39e55f36-85c6-4867-8229-a6df57bfc86b" />
  </p>
  <p align="center">
    <em>Screenshot highlighting the "Start capturing packets" icon within the Wireshark interface.</em>
  </p>

  <p align="center">
    <img width="1195" alt="Screenshot of Wireshark Capturing Traffic" src="https://github.com/user-attachments/assets/fdbf282f-3de2-4d58-948c-2f708df73f99" />
  </p>
  <p align="center">
    <em>Screenshot showing the Wireshark interface actively capturing network traffic.</em>
  </p>

---

3.  **Filter for ICMP Traffic and Ping Ubuntu VM:**
  * In the Wireshark filter bar, `icmp` was entered to display only ICMP packets.
  * Command Prompt was opened on the Windows 10 VM.
  * The `ping` command was used to send ICMP echo requests to the private IP address of the Ubuntu VM:
      ```
      ping <private IP address of linux-vm>
      ```

  <p align="center">
    <img width="894" alt="Screenshot of Command Prompt Pinging Ubuntu VM" src="https://github.com/user-attachments/assets/9c16b349-8ee1-4023-afa3-e6eb4f8f5b83" />
  </p>
  <p align="center">
    <em>Screenshot of the Windows 10 Command Prompt showing successful ping requests and replies to the Ubuntu VM's private IP address.</em>
  </p>

  <p align="center">
    <img width="994" alt="Screenshot of Wireshark Showing ICMP Traffic (Ping Requests and Replies)" src="https://github.com/user-attachments/assets/63085e7c-4785-4264-a506-74d7f5dbe68b" />
  </p>
  <p align="center">
    <em>Screenshot of Wireshark filtered for ICMP traffic, displaying both "Echo (ping) request" and "Echo (ping) reply" packets exchanged between the Windows and Ubuntu VMs.</em>
  </p>

---

4.  **Ping a Public Website:**
  * In the same Command Prompt window, the `ping` command was used to target a public website:
      ```
      ping 8.8.8.8
      ```
  * Wireshark was observed to capture the outgoing ICMP echo requests and the incoming echo replies from Google's servers.

  <p align="center">
    <img width="905" alt="Screenshot of Command Prompt Pinging www.google.com" src="https://github.com/user-attachments/assets/3c6bd62c-a502-4be7-8d0b-91bcc00eaf59" />
  </p>
  <p align="center">
    <em>Screenshot of the Windows 10 Command Prompt showing successful ping requests and replies to 8.8.8.8 (www.google.com).</em>
  </p>

---

## Part 3: Configuring a Firewall (Network Security Group) and Observing Traffic

This section demonstrates how Network Security Groups (NSGs) act as firewalls in Azure and how they affect network traffic.

1.  **Initiate Perpetual Ping to Ubuntu VM:**
  * In the Command Prompt on the Windows 10 VM, a continuous ping was initiated to the Ubuntu VM using the `-t` flag:
      ```
      ping -t <private IP address of linux-vm>
      ```

2.  **Disable Inbound ICMP Rule in Ubuntu VM's NSG:**
  * Navigated to the Azure portal and located the Network Security Group associated with the Ubuntu VM's network interface.
  * Within the NSG's "Inbound security rules", add a rule denying ICMP traffic.

  <p align="center">
    <img width="444" alt="Screenshot of Ubuntu VM's NSG Inbound Rules - ICMP Rule Deletion Confirmation" src="https://github.com/user-attachments/assets/b9c200ca-4a07-4eb4-828a-0d5923a97bb8" />
  </p>
  <p align="center">
    <em>Screenshot addint inbound security rule denying ICMP traffic.</em>
  </p>

  <p align="center">
    <img width="1291" alt="Screenshot of Ubuntu VM's NSG Inbound Rules - ICMP Rule Deleted" src="https://github.com/user-attachments/assets/1d784a36-cf41-4673-a9bf-410e123bf587" />
  </p>
  <p align="center">
    <em>Screenshot of the Ubuntu VM's Network Security Group after the ICMP inbound rule has been added</em>
  </p>

---

3.  **Observe Impact on ICMP Traffic:**
  * Returned to the Windows 10 VM and observed Wireshark. Only outgoing ICMP echo request packets were visible, with no incoming echo replies.
  * The Command Prompt running the continuous ping began displaying "Request timed out" messages, indicating that the ICMP packets were no longer reaching the Ubuntu VM or the replies were being blocked.

  <p align="center">
    <img width="1053" alt="Screenshot of Wireshark Showing Only Ping Requests and Command Prompt Showing Timeout" src="https://github.com/user-attachments/assets/1241dc2d-d0e5-42f4-a96b-bbdaaed09ffd" />
  </p>
  <p align="center">
    <em>Screenshot showing Wireshark capturing only ICMP echo requests and the Windows Command Prompt displaying "Request timed out" due to the blocked inbound ICMP traffic.</em>
  </p>

---

4.  **Re-enable Inbound ICMP Rule in Ubuntu VM's NSG:**
  * Navigated back to the Ubuntu VM's NSG in the Azure portal.
  * The ICMP inbound security rule was re-enabled (or a new rule allowing ICMP was created with appropriate settings).

  <p align="center">
    <img width="1294" alt="Screenshot of Ubuntu VM's NSG Inbound Rules - ICMP Rule Re-enabled" src="https://github.com/user-attachments/assets/f217e8d2-c2eb-45f1-94ba-58515018d663" />
  </p>
  <p align="center">
    <em>Screenshot of the Ubuntu VM's Network Security Group with the inbound rule for ICMP re-enabled.</em>
  </p>

---

5.  **Observe ICMP Traffic Resumption:**
  * Returned to the Windows 10 VM and observed Wireshark. Both ICMP echo request and reply packets were now visible again.
  * The Command Prompt running the continuous ping resumed receiving successful reply messages.

  <p align="center">
    <img width="1027" alt="Screenshot of Wireshark Showing Ping Requests and Replies Resumed" src="https://github.com/user-attachments/assets/23ee732c-d3e2-42ae-b34d-b54ce133801c" />
  </p>
  <p align="center">
    <em>Screenshot showing Wireshark again capturing both ICMP echo requests and replies, and the Windows Command Prompt displaying successful ping responses.</em>
  </p>

---

6.  **Observing Secure Shell (SSH) Traffic:**
  * Ensured Wireshark capture was running on the Windows 10 VM.
  * Applied a filter for SSH traffic using `tcp.port == 22` or `ssh`.
  * Opened PowerShell on the Windows 10 VM and initiated an SSH connection to the Ubuntu VM using its private IP address:
      ```powershell
      ssh labuser@<private IP address>
      ```

  <p align="center">
    <img width="975" alt="Screenshot of Wireshark Showing SSH Traffic Before Password Entry" src="https://github.com/user-attachments/assets/773f7bba-e263-4993-ada0-9d46281a94c2" />
  </p>
  <p align="center">
    <em>Screenshot of Wireshark filtered for SSH traffic, showing the initial TCP handshake and subsequent packets before the SSH password was entered.</em>
  </p>

  <p align="center">
    <img width="1069" alt="Screenshot of Wireshark Showing SSH Traffic After Password Entry and Command Input" src="https://github.com/user-attachments/assets/88435138-0501-4e93-b6bc-35b94b2ad7d9" />
  </p>
  <p align="center">
    <em>Screenshot of Wireshark displaying the continuous stream of TCP packets associated with the active SSH session, including traffic generated by typing commands.</em>
  </p>

---

7.  **Observing Dynamic Host Configuration Protocol (DHCP) Traffic:**
  * Applied a filter for DHCP traffic using `dhcp`.
  * Ran a shell script (.bat) to release and renew the IP address. Running the commands on Powershell directly didn't produce all 4 DHCP packets.
  
  <p align="center">
    <img width="1216" alt="Screenshot of Wireshark Showing Full DHCP Exchange (Discover, Offer, Request, ACK)" src="https://github.com/user-attachments/assets/4cb149e6-83ed-47b5-aa44-3b8fbce93268" />
  </p>
  <p align="center">
    <em>Screenshot of Wireshark displaying the complete DHCP exchange: DHCP Discover (client broadcast), DHCP Offer (server unicast), DHCP Request (client broadcast), and DHCP ACK (server unicast).</em>
  </p>

---

8.  **Observing Domain Name System (DNS) Traffic:**
  * Applied a filter for DNS traffic using `dns`.
  * Opened a Command Prompt window on the Windows 10 VM and used the `nslookup` command to query the IP addresses of `google.com` and `disney.com`:
      ```
      nslookup www.google.com
      nslookup www.disney.com
      ```

  <p align="center">
    <img width="1356" alt="Screenshot of Wireshark Showing DNS Query and Response for google.com" src="https://github.com/user-attachments/assets/f9512772-a819-4f87-8c84-831fb4094ffc" />
  </p>
  <p align="center">
    <em>Screenshot of Wireshark filtered for DNS traffic, showing the DNS query sent to the DNS server for `google.com` and the corresponding DNS response containing its IP addresses.</em>
  </p>

  <p align="center">
    <img width="1349" alt="Screenshot of Wireshark Showing DNS Query and Response for disney.com" src="https://github.com/user-attachments/assets/ab48b847-b8f0-4763-af55-6b662ccb8241" />
  </p>
  <p align="center">
    <em>Screenshot of Wireshark filtered for DNS traffic, showing the DNS query sent to the DNS server for `disney.com` and the corresponding DNS response containing its IP addresses.</em>
  </p>

---

9. **Observing Remote Desktop Protocol (RDP) Traffic:**
  * Applied a filter for RDP traffic using `tcp.port == 3389`.
  * With the Remote Desktop connection to the Windows 10 VM still active, observed the Wireshark capture.

  <p align="center">
    <img width="1091" alt="Screenshot of Wireshark Showing Continuous RDP Traffic" src="https://github.com/user-attachments/assets/31e38804-a683-463b-95d1-9e7e112aed09" />
  </p>
  <p align="center">
    <em>Screenshot of Wireshark filtered for TCP port 3389 (RDP), showing a continuous stream of packets.</em>
  </p>

  * **Observation:** A continuous stream of TCP packets on port 3389 was observed.
  * **Explanation:** This constant traffic occurs because RDP provides a persistent graphical interface stream from the remote computer to the local machine. Unlike request-response protocols like HTTP or DNS, RDP maintains an active connection, continuously transmitting updates for the display, user input, and other interactive elements.

<br />
