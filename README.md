# Network-Security-Homelab-pfSense-Snort-and-Active-Directory
This lab simulates a secure network environment using pfSense for firewall and routing, Snort for intrusion detection, and a Windows Server domain to manage devices. It provides hands-on experience with network security monitoring, traffic filtering, and domain administration.

This whole project was done on my laptop with Virtual box.
Refer to my documentation for for my official document for this project, I will just be reiterating here with some screenshots from my project.

Step 1: Creating the pfSense virtual machine.
  Creating this Virtual Machine is very straight forward so Ill skip over the details. You do want to make sure you have 2 network adapters. 1 for NAT and 2 for Internal Network, I named mine "intnet"
  When you have completed the installation, your should be looking at this screen:
  ![pfsense](https://github.com/user-attachments/assets/739b270c-f84b-4d5c-8335-9d286d7a52c9)


Step 2: Create Windows Server Virtrual machine.
  For my project, I used Windows Server 2022.
  Again, install is pretty straight forward, just be sure to set your network adapter to internal network as we did previously and give it the same name.
  Once install was complete, I changed the DNS ip setting so the server would point to itself and I also gave the server a static IP address as well
  ![server ip settings](https://github.com/user-attachments/assets/f76d5b7f-d1b3-41a0-bd94-220e13cf87b7)

  Next, I added the Active Directory Domain Service feature and promoted the server to a Domain Controller. I added a new forest and called my domain cxlebdomain.com
  From there, I began to add a few users via active directory just to help simulate a small corporate office environment.
  ![server ad screenshot](https://github.com/user-attachments/assets/4ec01f59-18e1-472d-8f9b-fa04c8151c23)


Step 3: Create Windows 10 Virtual Machine.
  Like all the previous steps, setting up this virtual machine is straight forward so Ill skip the details. Again set network adapter to internal network.
  Once installed, I changed the DNS ip settings to point to the Server I set up in the previous step, this will enable me to join the domain.
  ![windows 10 IP settings](https://github.com/user-attachments/assets/6e55c98a-0771-4d19-a891-965b37913d29)

  I joined the domain and once I had, I logged out of the local account and tried signing in with a domain account I created previously and the sign in was successful.

Step 4: Create Kali Linux Virtual Machine
  I already had a Kali Linux vm installed from previous experiements but install again is super simple, only thing I changed was the network adapter setting which I set to Internal Network
  After I changed the network adapter setting, I logged in and checked my IP address and pinged the pfSense to ensure proper connection.

Step 5: Install and configure Snort on pfSense
  I logged into the pfSense GUI by entering the ip address in a web browser on the Server. I logged in and clicked "Services" then "Packet Manager" then "Available Packages". I then searched and installed Snort, very straight forward. 
  I navigated to Services then Snort and added the LAN interface for Snort to monitor since I wanted to monitor internal network activity.
  ![lan interfect screenshot](https://github.com/user-attachments/assets/eecea969-6244-4367-968c-dc43e5bf3c7f)

  I configured rules in a way that would suit what I needed for my project but they would vary depending on what it was I wwas trying to accomplish. 
  I did add one custom rule: alert tcp any any -> 192.168.1.1/24 any (msg:"Nmap Scan Detected on LAN"; Flag: S; sid:1000002; rev:1;) specifically tailored to the nmap scans I was going to do to trigger alerts.

Step 6: Testing the network
  To test the network connectivity, I made sure I could ping the pfSense as well as access the internet from every machine.
  On my Windows 10 virtual machine, I made sure I could log into each domain account I had created.
  Lastly, from my Kali Linux machine playing the part of bad actor, I ran an Nmap scan on the server, and Snort identified it successfully!
  ![kali linux nmap](https://github.com/user-attachments/assets/3b0ca93c-ccec-472d-b808-174b206232d8)
  ![snort alert](https://github.com/user-attachments/assets/923713cb-1ca9-4558-bc55-af232a6d5977)
