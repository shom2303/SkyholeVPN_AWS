<h1>Skyhole VPN AWS</h1>

<h2>Description</h2>
In this project, I configured a self-hosted VPN and DNS filtering solution using Pi-hole, WireGuard, and Unbound on a Debian-based AWS Lightsail instance. WireGuard was set up as the VPN server to encrypt network traffic, while Pi-hole handled DNS-based ad blocking across connected devices. To enhance privacy and reduce third-party DNS reliance, I integrated Unbound as a recursive DNS resolver. This setup provides secure, encrypted browsing with customizable DNS filtering, showcasing my skills in Linux system administration, VPN architecture, and DNS privacy optimization.
<br />


<h2>Languages and Utilities Used</h2>

- <b>Bash</b>  
- <b>Pi-hole</b>  
- <b>Unbound</b>  
- <b>OpenVPN</b>  
- <b>Lighttpd</b>  
- <b>dig</b>  
- <b>curl</b>
- <b>nano</b> 

<h2>Environments Used</h2>

- <b>Apple macOS</b>  
- <b>Linux (Debian-based, AWS Lightsail)</b> 

<h2>Lab walk-through:</h2>

<p align="center">
Set up an AWS Account and go into Amazon Lightsail  <br/>
<img src="https://imgur.com/aeMrFSu.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Create an Instance and configure Network to have a static IPv4 and disable IPv6  <br/>
<img src="https://imgur.com/gHM0PAB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
SSH into the instance and update for newest packages and security updates. Use these commands: 
sudo apt-get update
sudo apt-get upgrade -y
sudo reboot  <br/>
<img src="https://imgur.com/x0JtS8P.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Install Pi-hole using the following command: sudo curl -sSL https://install.pi-hole.net | bash  <br/>
<img src="https://imgur.com/SvmNVZd.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
 <br />
Set up your VPN and choose between WireGuard and OpenVPN, and choose a port during set up using the following command: curl -L https://install.pivpn.io | bash     (port: 52222)  <br/>
<img src="https://imgur.com/hCMULRa.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
 <br />
Add a rule in Networking through UDP and the port you selected from VPN set up   <br/>
<img src="https://imgur.com/u1XqxGn.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
 <br />
Add a client and make a QR code using the following commands: 
pivpn add 
pivpn -qr
pivpn -l (to list clients)  <br/>
<img src="https://imgur.com/ScfIEK0.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
 <br />
Set up Pi-hole as your own recursive DNS server to cache your own queries. You first install unbound with the following command: sudo apt install unbound  <br/>
<img src="https://imgur.com/BgUOpdB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
 <br />
Then you go into the following directory and provide configuration to set up a local DNS resolver on 127.0.0.1:5335 for use with Pi-hole, enabling secure DNS resolution with DNSSEC (Domain Name System Security Extensions) validation, prefetching, and hardened privacy while blocking queries to private IP ranges with the following command: 

sudo nano /etc/unbound/unbound.conf.d/pi-hole.conf

And then the following configuration: 

server:

#If no logfile is specified, syslog is used

#logfile: "/var/log/unbound/unbound.log"

verbosity: 0

interface: 127.0.0.1
port: 5335
do-ip4: yes
do-udp: yes
do-tcp: yes

#May be set to no if you don't have IPv6 connectivity

do-ip6: yes

#You want to leave this to no unless you have *native* IPv6. With 6to4 and

#Terredo tunnels your web browser should favor IPv4 for the same reasons

prefer-ip6: no

#Use this only when you downloaded the list of primary root servers!

#If you use the default dns-root-data package, unbound will find it automatically

#root-hints: "/var/lib/unbound/root.hints"

#Trust glue only if it is within the server's authority
harden-glue: yes

#Require DNSSEC data for trust-anchored zones, if such data is absent, the zone becomes BOGUS
harden-dnssec-stripped: yes

#Don't use Capitalization randomization as it known to cause DNSSEC issues sometimes

#see https://discourse.pi-hole.net/t/unbound-stubby-or-dnscrypt-proxy/9378 for further details
use-caps-for-id: no

#Reduce EDNS reassembly buffer size.

#IP fragmentation is unreliable on the Internet today, and can cause

#transmission failures when large DNS messages are sent via UDP. Even

#when fragmentation does work, it may not be secure; it is theoretically

#possible to spoof parts of a fragmented DNS message, without easy

#detection at the receiving end. Recently, there was an excellent study

#>>> Defragmenting DNS - Determining the optimal maximum UDP response size for DNS <<<

#by Axel Koolhaas, and Tjeerd Slokker (https://indico.dns-oarc.net/event/36/contributions/776/)

#in collaboration with NLnet Labs explored DNS using real world data from the

#the RIPE Atlas probes and the researchers suggested different values for

#IPv4 and IPv6 and in different scenarios. They advise that servers should

#be configured to limit DNS messages sent over UDP to a size that will not

#trigger fragmentation on typical network links. DNS servers can switch

#from UDP to TCP when a DNS response is too big to fit in this limited

#buffer size. This value has also been suggested in DNS Flag Day 2020.

edns-buffer-size: 1232

#Perform prefetching of close to expired message cache entries

#This only applies to domains that have been frequently queried

prefetch: yes

#One thread should be sufficient, can be increased on beefy machines. In reality for most users
running on small networks or on a single machine, it should be unnecessary to seek performance
enhancement by increasing num-threads above 1.

num-threads: 1

#Ensure kernel buffer is large enough to not lose messages in traffic spikes

so-rcvbuf: 1m

#Ensure privacy of local IP ranges

private-address: 192.168.0.0/16
private-address: 169.254.0.0/16
private-address: 172.16.0.0/12
private-address: 10.0.0.0/8
private-address: fd00::/8
private-address: fe80::/10
  <br/>
<img src="https://imgur.com/DBIHzC6.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
 <br />
Restart Unbound Service with the following command: (sudo service unbound restart) and test if the Pi-hole can communicate to itself with the following command: (dig pi-hole.net @127.0.0.1 -p 5335)  <br/>
<img src="https://imgur.com/hfNbsX3.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
 <br />
Test validation that should give error message because it should not connect to anything using the following command: dig fail01.dnssec.works @127.0.0.1 -p 5335  <br/>
<img src="https://imgur.com/T8VGh7L.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
 <br />
Test validation that should connect and provide No Error using the following command: dig dnssec.works @127.0.0.1 -p 5335  <br/>
<img src="https://imgur.com/qNGnnRi.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
 <br />
Make a directory for scripts and you will put a script that will update everything with the following commands: 
mkdir scripts
touch scripts/update.sh
nano scripts/updates.sh

And add the following script: 

#!/bin/bash

#Update the package index

sudo apt-get update

#Upgrade the installed packages

sudo apt-get upgrade -y

#Update Pihole and Adblock List

sudo pihole updatePihole

sudo pihole updateGravity

#Clean APT

sudo apt autoclean -y
#Remove unneeded Packages

sudo apt autoremove -y  <br/>
<img src="https://imgur.com/RIKgfxZ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Give Execution Permissions to update.sh file with the following command: chmod +x scripts/update.sh

And then automate the process of updating by going into your crontab with the command: sudo crontab -e (then enter option 1)

Finally add the automation script towards the bottom of the cron page: 
#runs update.sh script to update/upgrade server and refresh Pihole adblock list
0 6 * * * /home/admin/scripts/update.sh
#reboots instance
30 6 * * * /sbin/shutdown -r  <br />
<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
