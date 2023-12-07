<h1 align="center">TryHackMe Tutorial</h1>
<h2 align="center">Skynet</h2>

Dedicated to all the legends on our Discord channel - *CyberSamurai*

<p align="center"> <img src="./images/1.jpg"></p>


<h1> Enumeration </h1>
<h3>NMAP</h3>
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

<h2>Port 80 (HTTP)</h2>
Navigating to the webserver using the IP Address of the target machine (in this case 10.10.177.196) on a browser of your choice, you get the below:<br>
<p align="center"> <img src="./images/3.png"></p><br>

This looks like a search engine, which does nothing when you search or when you click the buttons.<br>

Viewing the Page Source also does not disclose any information.

<h3>GOBUSTER</h3>

```bash
gobuster dir -u http://[targetIP]/ -w /usr/share/dirbuster/wordlists/directory-list-2.3-small.txt
```
<p align="center"> <img src="./images/4.png"></p><br>

**Hidden directories:**<br>
/admin<br>
/css<br>
/js<br>
/config<br>
/ai<br>
/squirrelmail<br>

If you had to navigate to the first 5 directories, you will find that all of them are Forbidden.<br>

<p align="center"> <img src="./images/5.png"></p><br>

However, */squirrelmail* brings up what looks like a webmail system (hence the open ports 110 and 143?):<br>
<p align="center"> <img src="./images/6.png"></p><br>

While there is nothing we can do just as yet, it conviently discloses the version SquirrelMail is running on. **Version 1.4.23**.

<h3>SEARCHSPLOIT</h3>

```bash
$ searchsploit squirrelmail
```
<p align="center"> <img src="./images/7.png"></p><br>

<p>Although there are some very important information regarding SquirrelMail vulnerabilities here, there are none that pique my interest since the version of SquirrelMail we are dealing with is not referenced anywhere.</p>

<p>For now, there is not much else we can do on the webserver.</p>

<p>Let's investigate port 139.</p>

<h2>Port 139 (SMB)</h2>

<h3>SMBCLIENT</h3>
Let's use smbclient to enumerate the SMB File Share system on the server.<br>

```bash
$ smbclient -L \\\\[targetIP]\\ 
```

<p align="center"> <img src="./images/8.png"></p><br>

Here we can see the different SMB shares currently on the server. There is also a possible username: **milesdyson**. Let's see what we can access.

**Anonymous**
<p>The first share that catches my attention is the anonymous share. This will allow us to enter without specifying a username or password.</p>

```bash
$ smbclient \\\\[targetIP]\\anonymous
```
*(notice the removal of the ```-L``` switch since we are now not listing the shares, but rather connecting to, in this case **anonymous** as specified on ```\\anonymous```)*








