<h1>WebStrike Lab - Analyzing a Suspicious File</h1>


<h2>Description</h2>

A suspicious file was identified on a company web server, raising alarms within the intranet. The Development team flagged the anomaly, suspecting potential malicious activity. To address the issue, the network team captured critical network traffic and prepared a PCAP file for review.
The task is to analyze the provided PCAP file to uncover how the file appeared and determine the extent of any unauthorized activity.
<br />


<h2>Tools and Utilities Used</h2>

- <b>Wireshark</b> 
- <b>What Is My IP Address</b>


<h2>Lab Walk-Through:</h2>

<p align="center">
First we start by heading to the Artifacts folder to retrieve the file: <br/>
<img src="https://imgur.com/DtfL0Bh.png" height="80%" width="80%" alt="Analyzing Suspicious File"/>
<br />
<br />
Once there, click on the WebStrike.pcap file to begin analysis:  <br/>
<img src="https://imgur.com/BXPq675.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
After we click on the file, we are taken to Wireshark where we can begin to search for malicious network activity: <br/>
<img src="https://imgur.com/4VJPENF.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
If we filter out the HTTP GET requests, we can identify that the source IP that caused this malicious activity comes from 117.11.88.124:  <br/>
<img src="https://imgur.com/AnbSIfk.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
The use of handy applications such as "What is My IP Address" can provide valuable information about the IP Address. From this information, we can assume that this IP originated from a datacenter in Tianjin, China. This information can assist with implementing geoblocking measures and analyzing threat intelligence:  <br/>
<img src="https://imgur.com/SHa1p42.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
If we take a deeper dive into the packets, we can also find the attacker's User Agent (Mozilla/5.0 (X11; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/115.0). Knowing this information can help with creating firewall filtering rules to defend against future attacks:  <br/>
<img src="https://imgur.com/RtWMHCH.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
To identify what files the attacker may have uploaded to exploit the web server, we can filter out network traffic to review POST requests:  <br/>
<img src="https://imgur.com/aBdNnr5.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
We can gather more information by following the HTTP stream:  <br/>
<img src="https://imgur.com/4eHPZ3p.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
From the HTTP stream, we can see that the attacker has uploaded a malicious file to the web server named "image.jpg.php". We can assume that this is a malicious file because it contains two extensions (jpg and php) which is a common tactic by malicious actors to hide the true file type:  <br/>
<img src="https://imgur.com/MTacCvq.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
To identify where this malicious file was uploaded and retrieved, we can filter out the network traffic to analyze the GET requests. We can see that this file is uploaded to the /reviews/uploads/ directory:  <br/>
<img src="https://imgur.com/CcIwZ57.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
We can go back to the back to our POST request filter and use the HTTP stream to analyze more information on the file upload:  <br/>
<img src="https://imgur.com/oEQzrE3.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
From this HTTP stream, we can also identify that this file upload gives the attacker shell capabilities on port 8080. This malicious actor is using a common listening tool, netcat, which gives the attacker the ability to execute commands by using a C2 server to establish a connection to the target server:  <br/>
<img src="https://imgur.com/rmKzhTr.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
To identify if the attacker exfiltrated any files to its C2 server, we can filter out the network traffic to gather TCP packets sent to port 8080, which we identified the attacker was using:  <br/>
<img src="https://imgur.com/qTpwy8Z.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
When we analyze the TCP stream we can see that the attacker uses commands such as whoami to verify that they have successfully gained access to the system. We can also see that they not only viewed the /etc/passwd/ file but also exfiltrated that file over the C2:  <br/>
<img src="https://imgur.com/EE70rMZ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/vFVEceW.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<h2>Best Practices and Remediation </h2>

- <b>When communicating over the network, always use secure protocols. Using HTTPS (443) rather than using HTTP (80) is a great way to keep communications secure.<b>
- <b>Use the principle of least privilege for administrator accounts on web servers to minimize the risk of an attacker gaining access.<b>
- <b>Ensure administrator accounts to critical systems have a strict password policy with regular monitoring and maintenance.<b>
- <b>Avoid putting sensitive data of user accounts in the /etc/passwd/ file because it is a typical attack vector for threat actors to perform attacks that allow lateral movement and privilege escalation.<b>
- <b>Continuously monitor for new patches and system upgrades to avoid exploitation of known vulnerabilities.<b>

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
