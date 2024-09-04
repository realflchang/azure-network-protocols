<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />

<!---
<h2>Video Demonstration</h2>

- ### [YouTube: Azure Virtual Machines, Wireshark, and Network Security Groups](https://www.youtube.com) -->

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

- Step 1 - create a VM running Windows 10 and another VM running Linux/Ubuntu. Note the private and public IP addresses.
- Step 2 - connect to the Windows VM
- Step 3 - install Wireshark on the Windows VM to help test our virtual network
- Step 4 - run some commands in Windows VM command prompt and observe results in Wireshark
- Step 5 - sign in to Ubuntu SSH and try to see if it can reach the Windows VM

<h2>Actions and Observations</h2>

<p>Begin by creating our 2 virtual machines (VM), 1 running Windows 10 aka "VM1" and 1 running Linux/Ubuntu aka "VM2":</p>
<img src="https://github.com/user-attachments/assets/5761c877-0d8d-4e80-a9f5-770b2efe1aab" alt="Created 2 VMs" />
<br /><br />

<p>Our Resource Group of the 2 VMs. Note this test is for both VMs in the same virtual network:</p>
<img src="https://github.com/user-attachments/assets/4d124a1d-a451-4944-8128-b332bc98c439" alt="Resource Group of the 2 VMs" />
<br /><br />

<p>Details of our VM1 running Windows 10. Note our private and public IP addresses:</p>
<img src="https://github.com/user-attachments/assets/7632cb1a-31b0-42a4-8f43-c02bac583427" alt="VM1 details" />
<br /><br />

<p>Details of our VM2 running Linux/Ubuntu. Note our private and public IP addresses:</p>
<img src="https://github.com/user-attachments/assets/ab0e095f-5b13-46f2-8568-1ace15ac910c" alt="VM2 details" />
<br /><br />

<p>Use Microsoft Remote Desktop Connection, with VM1's public IP address, we try to connect to the Windows VM:</p>
<img src="https://github.com/user-attachments/assets/29ed4288-6934-4fb1-b110-49affc0260d5" alt="RDC Login" />
<img src="https://github.com/user-attachments/assets/9890ba34-898f-4b1b-9d42-730655b15a5f" alt="Enter credentials" />
<br /><br />

<p>Established connection to the Windows VM:</p>
<img src="https://github.com/user-attachments/assets/ac85a143-e015-47cc-923e-1004f3e77edd" alt="Established connection to Windows VM" />
<br /><br />

<p>Next using the Windows VM, we go to Wireshark's website, download and install the network tool:</p>
<img src="https://github.com/user-attachments/assets/9161829b-e732-4a71-9c5b-67c6895d9ec2" alt="Installed Wireshark" />
<br /><br />

<p>We are ready to test. First test is to test ICMP protocol, the protocol used by Ping:</p>
<img src="https://github.com/user-attachments/assets/3ef339d5-79f0-4ba1-8596-de702d4c0d30" alt="Ready to test Ping with Wireshark" />
<br /><br />

<p>Run command prompt, and try pinging to our Ubuntu VM using its private IP address (10.0.0.5). :</p>
<img src="https://github.com/user-attachments/assets/31f32438-e962-4d1e-8ec9-842f34cbef24" alt="Pinging to Ubuntu" />
<br /><br />

<p>Next test is to test TCP port 22, the port number used by SSH.  Here I will use SSH to sign in to our Ubuntu VM:</p>
<img src="https://github.com/user-attachments/assets/66105ed6-2116-4958-8f9b-00b340caee9a" alt="SSH to Ubuntu" />
<br /><br />

<p>Here I am running other commands in the SSH to the Ubuntu VM:</p>
<img src="https://github.com/user-attachments/assets/f7529aed-1379-45a2-95e6-706072c8db21" alt="Other commands in SSH to Ubuntu" />
<br /><br />

<p>If I put in TCP port 3389, which is the RDP port, Wireshark will display nonstop results since the Remote Desktop Connection session is active:</p>
<img src="https://github.com/user-attachments/assets/3fe6b969-0dfa-41a1-8ac1-64b3b5d007e1" alt="Testing TCP port 3389 RDP" />
<br /><br />

<p>For the next test, we will continually ping the Ubuntu VM, then in Azure Network Security Group Inbound settings try blocking the connection, and see our results in Wireshark:</p>
<img src="https://github.com/user-attachments/assets/91c6e031-236f-4b03-aa39-1d0a1857a448" alt="VM2 Inbound security rules" />
<br /><br />

<p>Set the Action to "Deny" for ICMP protocol, so this should block the ping to the Ubuntu VM:</p>
<img src="https://github.com/user-attachments/assets/2caba882-6199-4542-a2cc-cc55e8564a9d" alt="Deny ICMP inbound to Ubuntu VM" />
<br /><br />

<p>After adding the setting in Azure Network Security Group Inbound Security Rules for Ubuntu VM:</p>
<img src="https://github.com/user-attachments/assets/f9c02d9a-cf3e-42d2-9499-88aa543c0518" alt="Result of the NSG Inbound for Ubuntu VM" />
<br /><br />

<p>Results of ping and Wireshark. As expected, ping starts to time-out because of the inbound rule, and Wireshark also does not receive "replies":</p>
<img src="https://github.com/user-attachments/assets/2d072ff2-aa3b-4df0-a17f-c5846884735e" alt="Wireshark and ping results as expected" />
<br /><br />

<p>Let's change the inbound setting to allow ICMP and ping again to the Ubuntu VM:</p>
<img src="https://github.com/user-attachments/assets/e4e56944-e676-43d4-a17c-73e9dde5e2b9" alt="Allow ICMP and ping again in NSG" />
<br /><br />

<p>As expected, ping and Wireshark showing results again:</p>
<img src="https://github.com/user-attachments/assets/9a064bc3-82b1-44e6-9eae-18e18dbe31a5" alt="Ping and Wireshark showing results again" />
<br /><br />

<p>Let's do a reverse ping from the Ubuntu VM to our Windows VM.  By default, Windows disables incoming ICMP commands, so let's enable first by going to Windows Firewall, Inbound Rules, :</p>
<img src="https://github.com/user-attachments/assets/21fd5f0e-67a7-4a0b-bfb4-cda4ad2c163c" alt="Windows Firewall showing ICMP deny as default" />
<br /><br />

<p>Right click on "File and Printer Sharing (Echo Request ICMPv4-in) and change to "Enable Rule":</p>
<img src="https://github.com/user-attachments/assets/6ea01db7-7fa4-4942-97a2-e02882eb6216" alt="Change to Enable Rule" />
<br /><br />

<p>Windows Firewall now allows the ICMP inbound rule:</p>
<img src="https://github.com/user-attachments/assets/64d524b5-e28b-4416-b93c-8a131506ed71" alt="Allow ICMP inbound rule" />
<br /><br />

<p>From the SSH to our Ubuntu VM, we can run a ping command and see the results.  (I actually installed ping separately in the Ubuntu VM to allow the command to run):</p>
<img src="https://github.com/user-attachments/assets/94dca37e-bf7b-402d-9eca-061ab1629d1f" alt="Ping from Ubuntu VM to Windows VM" />
<br /><br />

<p>This confirms both VMs could communicate with each other in the same virtual network.</p>


