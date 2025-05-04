TOZED ZLT S12 PRO OpenWRT Configuration Backups
This repository contains configuration backups for the TOZED ZLT S12 PRO router running OpenWRT. The backups are provided as .zip files and include configurations for two distinct setups:

Wi-Fi Repeater: Connects to an upstream Wi-Fi router via the 2.4 GHz radio and broadcasts the internet via the 5 GHz radio.
Ethernet to Wi-Fi Access Point: Receives internet via an Ethernet (LAN) cable with a static IP configuration and broadcasts it via Wi-Fi.

📂 Repository Structure

/wifi-repeater/: Contains the .zip backup file for the Wi-Fi repeater configuration.
/ethernet-to-wifi/: Contains the .zip backup file for the Ethernet to Wi-Fi access point configuration.
README.md: This file, providing setup instructions and details.

🧰 Requirements

TOZED ZLT S12 PRO router with OpenWRT installed.
A computer to access the OpenWRT LuCI web interface (http://192.168.1.1).
For the Ethernet setup: A LAN cable connected to an upstream network with static IP details.
For the Wi-Fi repeater setup: Access to an upstream Wi-Fi network.

⚙️ Configuration Details
1. Wi-Fi Repeater Setup
This configuration enables the router to connect to an upstream Wi-Fi network (via the 2.4 GHz radio) and broadcast the internet via the 5 GHz radio.
Steps to Configure

Access LuCI:

Connect to the router via LAN or Wi-Fi.
Open a browser and navigate to http://192.168.1.1.
Log in to the LuCI interface.


Configure 2.4 GHz Radio (Client Mode):

Navigate to Network > Wireless.
Edit the 2.4 GHz radio and set:Mode       : Client
Network    : wwan (create a new interface if needed)
SSID       : (Upstream Wi-Fi SSID)
Encryption : (Match upstream Wi-Fi encryption, e.g., WPA2-PSK)
Password   : (Upstream Wi-Fi password)


Save & Apply.


Configure 5 GHz Radio (Access Point):

Edit the 5 GHz radio and set:Mode       : Access Point
Network    : wwan
SSID       : (Your desired Wi-Fi name)
Encryption : WPA2-PSK or WPA3
Password   : (Your desired password)


Save & Apply.


Firewall Configuration:

Navigate to Network > Firewall.
Assign the wwan interface to the WAN zone.
Ensure LAN → WAN forwarding is enabled:LAN (Zone): Input: accept | Output: accept | Forward: accept
WAN (Zone): Input: reject | Output: accept | Forward: reject
Forwardings: Allow forward from lan to wan




Test the Setup:

Connect a device to the 5 GHz Wi-Fi network.
Verify internet access using:ping 8.8.8.8
ping google.com





Backup File

Located in /wifi-repeater/.
Restore via LuCI: System > Backup / Flash Firmware > Restore.


2. Ethernet to Wi-Fi Access Point Setup
This configuration enables the router to receive internet via an Ethernet (LAN) cable with a static IP and broadcast it via Wi-Fi.
Static IP Details
IP Address     : 10.50.52.22
Subnet Mask    : 255.255.255.0
Default Gateway: 10.50.52.254
DNS Server     : 10.50.2.254

Steps to Configure

Access LuCI:

Connect to the router via LAN.
Open a browser and navigate to http://192.168.1.1.
Log in to the LuCI interface.


Remove LAN Port from Bridge:

Navigate to Network > Interfaces > Devices.
Edit the br-lan bridge device.
Remove the Ethernet port (eth0) from the bridge.
Save & Apply.


Create a New WAN Interface:

Navigate to Network > Interfaces.
Click Add new interface... and configure:Name       : wan_lan
Protocol   : Static address
Device     : eth0


Under General Settings, set:IPv4 address: 10.50.52.22
Netmask     : 255.255.255.0
Gateway     : 10.50.52.254
DNS         : 10.50.2.254


Save & Apply.


Configure Wi-Fi as Access Point:

Navigate to Network > Wireless.
Edit the Wi-Fi interface and set:Mode       : Access Point
Network    : wan_lan
SSID       : (Your Wi-Fi name)
Encryption : WPA2-PSK or WPA3
Password   : (Your password)


Save & Apply.


Firewall Configuration:

Navigate to Network > Firewall.
Assign the wan_lan interface to the WAN zone.
Ensure LAN → WAN forwarding is enabled:LAN (Zone): Input: accept | Output: accept | Forward: accept
WAN (Zone): Input: reject | Output: accept | Forward: reject
Forwardings: Allow forward from lan to wan




Test the Setup:

Connect the internet-enabled LAN cable to the Ethernet port.
Connect a device to the configured Wi-Fi network.
Verify internet access using:ping 8.8.8.8
ping google.com


Access the OpenWRT interface via http://10.50.52.22.



Backup File

Located in /ethernet-to-wifi/.
Restore via LuCI: System > Backup / Flash Firmware > Restore.

📝 Notes

Ensure you have the correct Ethernet port (eth0) identified for your device.
Backup your current configuration before restoring any .zip files.
For troubleshooting, check the OpenWRT logs via Status > System Log or use SSH to access the router.
The configurations assume a clean OpenWRT installation. Adjust settings if your setup includes additional customizations.
