<h1>Skyhole VPN AWS</h1>

 ### [YouTube Demonstration](https://youtu.be/VmLbdTw0HX8)

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
Identified the source IP address initiating suspicious requests through packet analysis: <br/>
<img src="https://imgur.com/aeMrFSu.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Determined the country of origin for the attacker based on the identified IP address:  <br/>
<img src="https://imgur.com/gHM0PAB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Found the open port that grants access to the web server’s admin panel:  <br/>
<img src="https://imgur.com/x0JtS8P.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Analyzed the tools used by the attacker for directory and file enumeration:  <br/>
<img src="https://imgur.com/SvmNVZd.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
 <br />
Identified the specific admin panel directory uncovered by the attacker:  <br/>
<img src="https://imgur.com/hCMULRa.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
 <br />
Located the hash value used for login credentials in the captured traffic:  <br/>
<img src="https://imgur.com/u1XqxGn.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
 <br />
Used CyberChef to decode the hash value into plaintext username and password:  <br/>
<img src="https://imgur.com/ScfIEK0.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
 <br />
Detected the malicious file uploaded by the attacker to establish a reverse shell:  <br/>
<img src="https://imgur.com/BgUOpdB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
 <br />
Analyzed the command scheduled by the attacker to maintain persistence on the server:  <br/>
<img src="https://imgur.com/DBIHzC6.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
 <br />
Analyzed the command scheduled by the attacker to maintain persistence on the server:  <br/>
<img src="https://imgur.com/hfNbsX3.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
 <br />
Analyzed the command scheduled by the attacker to maintain persistence on the server:  <br/>
<img src="https://imgur.com/T8VGh7L.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
 <br />
Analyzed the command scheduled by the attacker to maintain persistence on the server:  <br/>
<img src="https://imgur.com/qNGnnRi.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
 <br />
Analyzed the command scheduled by the attacker to maintain persistence on the server:  <br/>
<img src="https://imgur.com/RIKgfxZ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
