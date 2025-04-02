<h1>WebStrike Lab - Analyzing a Suspicious File</h1>


<h2>Description</h2>

A suspicious file was identified on a company web server, raising alarms within the intranet. The Development team flagged the anomaly, suspecting potential malicious activity. To address the issue, the network team captured critical network traffic and prepared a PCAP file for review.
The task is to analyze the provided PCAP file to uncover how the file appeared and determine the extent of any unauthorized activity.
<br />


<h2>Tools and Utilities Used</h2>

- <b>Wireshark</b> 
- <b>What is My IP Address</b>

<h2>Environments Used </h2>

- <b>Windows 10</b> (21H2)

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
Observe the wiped disk:  <br/>
<img src="https://i.imgur.com/AeZkvFQ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
