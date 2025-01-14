<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this project, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />




<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 24.04

<h2>High-Level Steps</h2>

- Create a Resource Group
- Create a Virtual Machine
- Observe ICMP Traffic
- Observe SSH Traffic
- Observe DHCP Traffic
- Observe DNS Traffic
- Observe RDP Traffic

<h2>Actions and Observations</h2>
</br>
</br>
<h3 align="center">
  Set up your virtual environment
</h3>
</br>
<p>
  First, let's create our Resource Group inside our Azure subscription.
</p>
<p>
  <img src="https://i.imgur.com/8uluq6L.png" height="75%" width="100%" alt="Resource Group"/>
</p>
<p>
  Now create your Windows virtual machine. I typically create the VM in (US) West US.
</p>
<p>
  While creating the VM, select the previously created Resource Group and allow it to create a new Virtual Network (Vnet) and Subnet. Make sure to use the password option under the <strong>Administrator Account</strong> section:
</p>
<p>
  <img src="https://i.imgur.com/8NVbWQR.png" height="75%" width="100%" alt="Windows VM"/>
</p>
<p>
  Create an Ubuntu virtual machine.
</p>
<p>
  While creating the VM, select the previously created Resource Group and allow it to create a new Virtual Network (Vnet) and Subnet. Make sure to use the password option under the <strong>Administrator Account</strong> section (not seen in image):
</p>
<p>
  <img src="https://i.imgur.com/7P7aHLg.png" height="75%" width="100%" alt="Ubuntu VM"/>
</p>
<p>
  Observe Your Virtual Machines in the Virtual Machines tab:
</p>
<p>
  <img src="https://i.imgur.com/hgAzjUc.png" height="75%" width="100%" alt="Network Watcher"/>
</p>
<br />
<br />
<h3 align="center">
  Now let's observe some ICMP traffic
</h3>
<br />
<p>
  Open remote desktop, and log into your Windows 10 Virtual Machine using the public IP Address.
</p>
<p>
  <img src="https://i.imgur.com/8MLJFRH.png" height="75%" width="100%" alt="Microsoft Remote Desktop - Mac"/>
</p>
<p>
  Once logged in, download Wireshark for Windows.
</p>
<p>
  <img src="https://i.imgur.com/6qmIlO3.png" height="75%" width="100%" alt="Microsoft Remote Desktop - Mac"/>
</p>
<p>
  Open Wireshark, select "Ethernet," and click the blue shark icon to observe network traffic.
</p>
<p>
  <img src="https://i.imgur.com/FzWkTLS.png" height="75%" width="100%" alt="Microsoft Remote Desktop - Mac"/>
</p>
<p>
  You will see a lot of packets of different protocols, filter for ICMP.
</p>
<p>
  <img src="https://i.imgur.com/bQvpTnJ.png" height="75%" width="100%" alt="Microsoft Remote Desktop - Mac"/>
</p>
<p>
  You won't see any packets because ICMP packets are not being generated or transmitted on the network you are monitoring yet.
</p>
<p>
  <img src="https://i.imgur.com/z3ojiYG.png" height="75%" width="100%" alt="Microsoft Remote Desktop - Mac"/>
</p>
<p>
  Retrieve the private IP address of the Ubuntu VM and attempt to ping it from within the Windows 10 VM. Observe ping requests and replies within WireShark:
</p>
<p>
  <img src="https://i.imgur.com/fzaVRLP.png" height="75%" width="100%" alt="Ubuntu private IP"/>
  <img src="https://i.imgur.com/3nqPwMV.png" height="75%" width="100%" alt="ICMP traffic - private IP"/>
</p>
<p>
  Initiate a perpetual/non-stop ping from your Windows 10 VM to your Ubuntu VM:
</p>
<p>
  <img src="https://i.imgur.com/d3Au3lv.png" height="75%" width="100%" alt="ICMP traffic - perpetual ping"/>
</p>
<p>
  Open the Network Security Group your Ubuntu VM is using and disable incoming (inbound) ICMP traffic, while back in the Windows 10 VM, observe the ICMP traffic in WireShark and the command line Ping activity:
</p>
<p>
  <img src="https://i.imgur.com/ovGk5dq.png" height="75%" width="100%" alt="ICMP traffic - perpetual ping"/>
  <img src="https://i.imgur.com/dDwRC1P.png" height="75%" width="100%" alt="ICMP traffic - ICMP denied"/>
</p>
<p>
  Re-enable ICMP traffic for the Network Security Group in your Ubuntu VM and back in the Windows 10 VM, observe the ICMP traffic in WireShark and the command line ping activity (should start working again).Finally, stop the ping activity by pressing CTRL-C in Powershell.
</p>
<p>
  <img src="https://i.imgur.com/E9lzpRb.png" height="75%" width="100%" alt="ICMP traffic - ICMP re-enabled"/>
</p>
<br />
<br />
<h3 align="center">
  Time to observe SSH traffic
</h3>
<br />
<p>
  Now filter to only show SSH traffic in Wireshark, and SSH into VM2 using PowerShell:
</p>
</p>
<img src="https://i.imgur.com/kxcm0Oo.png" height="75%" width="100%" alt="SSH traffic"/>
</p>
</p>
  Exit the SSH connection by typing ‘exit’ and pressing [enter]:
</p>
  
<p>
<br />
<br />
<h3 align="center">
  Next, we're going to observe DHCP Traffic
</h3>
<br />
<p>
  Back in Wireshark, filter for DHCP traffic only. From your Windows 10 VM, attempt to issue your VM a new IP address from the command line (ipconfig /renew)
</p>
Observe the DHCP traffic appearing in WireShark:
</p>
<p>
  <img src="https://i.imgur.com/mKyAHFr.png" height="75%" width="100%" alt="DHCP traffic"/>
</p>
<br />
<br />
<h3 align="center">
  Let's now observe our DNS traffic next
</h3>
<br />
<p>
  Back in Wireshark, filter for DNS traffic only.
</p>
<p>
  From your Windows 10 VM within a command line, use nslookup to see what google.com and disney.com’s IP addresses are and observe the DNS traffic being shown in WireShark:
</p>
<p>
  <img src="https://i.imgur.com/mYZ8CAK.png" height="75%" width="100%" alt="DNS traffic"/>
</p>
<br />
<br />
<h3 align="center">
  Finally, we will observe RDP traffic to finish up this tutorial
</h3>
<br />
<p>
  Back in Wireshark, filter for RDP traffic only using "tcp.port==3389".
</p>
<p>
  You'll be obseving a non-stop stream of traffic. Do you know why there is constant traffic in our tcp.port==3389?
</p>
<p>
  The answer is because the RDP (protocol) is constantly showing you a live stream from one computer to another, therefor traffic is always being transmitted:
</p>
<p>
  <img src="https://i.imgur.com/hNlhTVp.png" height="75%" width="100%" alt="RDP traffic"/>
</p>
<p>
  Now that we're finished observing the network, DON'T FORGET TO CLEAN UP YOUR AZURE ENVIRONMENT! This will prevent you from incurring additional charges and you won't be left surprised!
</p>
<p>
  Close your Remote Desktop connection, delete the Resource Group(s) created at the beginning of this tutorial, and verify Resource Group deletion. You'll typically be notified or can click unde the bell notification just to make sure.
</p>
</p>

