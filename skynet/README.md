<h1 align="center">TryHackMe Tutorial</h1>
<h2 align="center">Skynet</h2>

Dedicated to all the legends on our Discord channel - *CyberSamurai*

<p align="center"> <img src="./images/1.jpg"></p>


<h1> Enumeration </h1>
<h2>NMAP</h2>
<p>First, we start with the CLASSIC nmap scan</p>

```bash
$ nmap -T4 -p- -A [targetIP]
```
*[targetIP] being the IP of Skynet CTF*
<p align="center"> <img src="./images/2.png"></p>

**Open Ports:**<br>
22/tcp  open  ssh         OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)<br>
80/tcp  open  http        Apache httpd 2.4.18 ((Ubuntu))<br>
110/tcp open  pop3        Dovecot pop3d<br>
139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)<br>
143/tcp open  imap        Dovecot imapd<br>
445/tcp open  netbios-0   Samba smbd 4.3.11-Ubuntu (workgroup: WORKGROUP)<br><br>

Okay, so we have webserver being hosted on port 80, OpenSSH on port 22 and Samba File Share on port 139/445. Ports 110 and port 143 do not interest me just as yet. We'll see.

Let's investigate port 80 first.<br><br>

<h3>Port 80 (HTTP)</h3>
Navigating to the webserver using the IP Address of the target machine (in this case 10.10.177.196) on a browser of your choice, you get the below:



