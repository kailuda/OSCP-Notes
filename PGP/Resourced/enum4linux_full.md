enum4linux-ng resourced.pgp                                      
ENUM4LINUX - next generation (v1.3.7)

 ==========================
|    Target Information    |
 ==========================
[*] Target ........... resourced.pgp
[*] Username ......... ''
[*] Random Username .. 'ewclzvgk'
[*] Password ......... ''
[*] Timeout .......... 5 second(s)

 ======================================
|    Listener Scan on resourced.pgp    |
 ======================================
[*] Checking LDAP
[+] LDAP is accessible on 389/tcp
[*] Checking LDAPS
[+] LDAPS is accessible on 636/tcp
[*] Checking SMB
[+] SMB is accessible on 445/tcp
[*] Checking SMB over NetBIOS
[+] SMB over NetBIOS is accessible on 139/tcp

 =====================================================
|    Domain Information via LDAP for resourced.pgp    |
 =====================================================
[*] Trying LDAP
[+] Appears to be root/parent DC
[+] Long domain name is: resourced.local

 ============================================================
|    NetBIOS Names and Workgroup/Domain for resourced.pgp    |
 ============================================================
[-] Could not get NetBIOS names information via 'nmblookup': timed out

 ==========================================
|    SMB Dialect Check on resourced.pgp    |
 ==========================================
[*] Trying on 445/tcp
[+] Supported dialects and settings:
Supported dialects:                                                                                                                                                                                                                         
  SMB 1.0: false                                                                                                                                                                                                                            
  SMB 2.0.2: true                                                                                                                                                                                                                           
  SMB 2.1: true                                                                                                                                                                                                                             
  SMB 3.0: true                                                                                                                                                                                                                             
  SMB 3.1.1: true                                                                                                                                                                                                                           
Preferred dialect: SMB 3.0                                                                                                                                                                                                                  
SMB1 only: false                                                                                                                                                                                                                            
SMB signing required: true                                                                                                                                                                                                                  

 ============================================================
|    Domain Information via SMB session for resourced.pgp    |
 ============================================================
[*] Enumerating via unauthenticated SMB session on 445/tcp
[+] Found domain information via SMB
NetBIOS computer name: RESOURCEDC                                                                                                                                                                                                           
NetBIOS domain name: resourced                                                                                                                                                                                                              
DNS domain: resourced.local                                                                                                                                                                                                                 
FQDN: ResourceDC.resourced.local                                                                                                                                                                                                            
Derived membership: domain member                                                                                                                                                                                                           
Derived domain: resourced                                                                                                                                                                                                                   

 ==========================================
|    RPC Session Check on resourced.pgp    |
 ==========================================
[*] Check for anonymous access (null session)
[+] Server allows authentication via username '' and password ''
[*] Check for guest access
[-] Could not establish guest session: STATUS_LOGON_FAILURE

 ====================================================
|    Domain Information via RPC for resourced.pgp    |
 ====================================================
[+] Domain: resourced
[+] Domain SID: S-1-5-21-537427935-490066102-1511301751
[+] Membership: domain member

 ================================================
|    OS Information via RPC for resourced.pgp    |
 ================================================
[*] Enumerating via unauthenticated SMB session on 445/tcp
[+] Found OS information via SMB
[*] Enumerating via 'srvinfo'
[-] Could not get OS info via 'srvinfo': STATUS_ACCESS_DENIED
[+] After merging OS information we have the following result:
OS: Windows 10, Windows Server 2019, Windows Server 2016                                                                                                                                                                                    
OS version: '10.0'                                                                                                                                                                                                                          
OS release: '1809'                                                                                                                                                                                                                          
OS build: '17763'                                                                                                                                                                                                                           
Native OS: not supported                                                                                                                                                                                                                    
Native LAN manager: not supported                                                                                                                                                                                                           
Platform id: null                                                                                                                                                                                                                           
Server type: null                                                                                                                                                                                                                           
Server type string: null                                                                                                                                                                                                                    

 ======================================
|    Users via RPC on resourced.pgp    |
 ======================================
[*] Enumerating users via 'querydispinfo'
[+] Found 13 user(s) via 'querydispinfo'
[*] Enumerating users via 'enumdomusers'
[+] Found 13 user(s) via 'enumdomusers'
[+] After merging user results we have 13 user(s) total:
'1103':                                                                                                                                                                                                                                     
  username: M.Mason                                                                                                                                                                                                                         
  name: (null)                                                                                                                                                                                                                              
  acb: '0x00020010'                                                                                                                                                                                                                         
  description: Ex IT admin                                                                                                                                                                                                                  
'1104':                                                                                                                                                                                                                                     
  username: K.Keen                                                                                                                                                                                                                          
  name: (null)                                                                                                                                                                                                                              
  acb: '0x00020010'                                                                                                                                                                                                                         
  description: Frontend Developer                                                                                                                                                                                                           
'1105':                                                                                                                                                                                                                                     
  username: L.Livingstone                                                                                                                                                                                                                   
  name: (null)                                                                                                                                                                                                                              
  acb: '0x00000210'                                                                                                                                                                                                                         
  description: SysAdmin                                                                                                                                                                                                                     
'1106':                                                                                                                                                                                                                                     
  username: J.Johnson                                                                                                                                                                                                                       
  name: (null)                                                                                                                                                                                                                              
  acb: '0x00020010'                                                                                                                                                                                                                         
  description: Networking specialist                                                                                                                                                                                                        
'1107':                                                                                                                                                                                                                                     
  username: V.Ventz                                                                                                                                                                                                                         
  name: (null)                                                                                                                                                                                                                              
  acb: '0x00000210'                                                                                                                                                                                                                         
  description: 'New-hired, reminder: HotelCalifornia194!'                                                                                                                                                                                   
'1108':                                                                                                                                                                                                                                     
  username: S.Swanson                                                                                                                                                                                                                       
  name: (null)                                                                                                                                                                                                                              
  acb: '0x00020010'                                                                                                                                                                                                                         
  description: Military Vet now cybersecurity specialist                                                                                                                                                                                    
'1109':                                                                                                                                                                                                                                     
  username: P.Parker                                                                                                                                                                                                                        
  name: (null)                                                                                                                                                                                                                              
  acb: '0x00020010'                                                                                                                                                                                                                         
  description: Backend Developer                                                                                                                                                                                                            
'1110':                                                                                                                                                                                                                                     
  username: R.Robinson                                                                                                                                                                                                                      
  name: (null)                                                                                                                                                                                                                              
  acb: '0x00020010'                                                                                                                                                                                                                         
  description: Database Admin                                                                                                                                                                                                               
'1111':                                                                                                                                                                                                                                     
  username: D.Durant                                                                                                                                                                                                                        
  name: (null)                                                                                                                                                                                                                              
  acb: '0x00020010'                                                                                                                                                                                                                         
  description: Linear Algebra and crypto god                                                                                                                                                                                                
'1112':                                                                                                                                                                                                                                     
  username: G.Goldberg                                                                                                                                                                                                                      
  name: (null)                                                                                                                                                                                                                              
  acb: '0x00020010'                                                                                                                                                                                                                         
  description: Blockchain expert                                                                                                                                                                                                            
'500':                                                                                                                                                                                                                                      
  username: Administrator                                                                                                                                                                                                                   
  name: (null)                                                                                                                                                                                                                              
  acb: '0x00000210'                                                                                                                                                                                                                         
  description: Built-in account for administering the computer/domain                                                                                                                                                                       
'501':                                                                                                                                                                                                                                      
  username: Guest                                                                                                                                                                                                                           
  name: (null)                                                                                                                                                                                                                              
  acb: '0x00000215'                                                                                                                                                                                                                         
  description: Built-in account for guest access to the computer/domain                                                                                                                                                                     
'502':                                                                                                                                                                                                                                      
  username: krbtgt                                                                                                                                                                                                                          
  name: (null)                                                                                                                                                                                                                              
  acb: '0x00020011'                                                                                                                                                                                                                         
  description: Key Distribution Center Service Account                                                                                                                                                                                      

 =======================================
|    Groups via RPC on resourced.pgp    |
 =======================================
[*] Enumerating local groups
[+] Found 5 group(s) via 'enumalsgroups domain'
[*] Enumerating builtin groups
[+] Found 28 group(s) via 'enumalsgroups builtin'
[*] Enumerating domain groups
[+] Found 15 group(s) via 'enumdomgroups'
[+] After merging groups results we have 48 group(s) total:
'1101':                                                                                                                                                                                                                                     
  groupname: DnsAdmins                                                                                                                                                                                                                      
  type: local                                                                                                                                                                                                                               
'1102':                                                                                                                                                                                                                                     
  groupname: DnsUpdateProxy                                                                                                                                                                                                                 
  type: domain                                                                                                                                                                                                                              
'498':                                                                                                                                                                                                                                      
  groupname: Enterprise Read-only Domain Controllers                                                                                                                                                                                        
  type: domain                                                                                                                                                                                                                              
'512':                                                                                                                                                                                                                                      
  groupname: Domain Admins                                                                                                                                                                                                                  
  type: domain                                                                                                                                                                                                                              
'513':                                                                                                                                                                                                                                      
  groupname: Domain Users                                                                                                                                                                                                                   
  type: domain                                                                                                                                                                                                                              
'514':                                                                                                                                                                                                                                      
  groupname: Domain Guests                                                                                                                                                                                                                  
  type: domain                                                                                                                                                                                                                              
'515':                                                                                                                                                                                                                                      
  groupname: Domain Computers                                                                                                                                                                                                               
  type: domain                                                                                                                                                                                                                              
'516':                                                                                                                                                                                                                                      
  groupname: Domain Controllers                                                                                                                                                                                                             
  type: domain                                                                                                                                                                                                                              
'517':                                                                                                                                                                                                                                      
  groupname: Cert Publishers                                                                                                                                                                                                                
  type: local                                                                                                                                                                                                                               
'518':                                                                                                                                                                                                                                      
  groupname: Schema Admins                                                                                                                                                                                                                  
  type: domain                                                                                                                                                                                                                              
'519':                                                                                                                                                                                                                                      
  groupname: Enterprise Admins                                                                                                                                                                                                              
  type: domain                                                                                                                                                                                                                              
'520':                                                                                                                                                                                                                                      
  groupname: Group Policy Creator Owners                                                                                                                                                                                                    
  type: domain                                                                                                                                                                                                                              
'521':                                                                                                                                                                                                                                      
  groupname: Read-only Domain Controllers                                                                                                                                                                                                   
  type: domain                                                                                                                                                                                                                              
'522':                                                                                                                                                                                                                                      
  groupname: Cloneable Domain Controllers                                                                                                                                                                                                   
  type: domain                                                                                                                                                                                                                              
'525':                                                                                                                                                                                                                                      
  groupname: Protected Users                                                                                                                                                                                                                
  type: domain                                                                                                                                                                                                                              
'526':                                                                                                                                                                                                                                      
  groupname: Key Admins                                                                                                                                                                                                                     
  type: domain                                                                                                                                                                                                                              
'527':                                                                                                                                                                                                                                      
  groupname: Enterprise Key Admins                                                                                                                                                                                                          
  type: domain                                                                                                                                                                                                                              
'544':                                                                                                                                                                                                                                      
  groupname: Administrators                                                                                                                                                                                                                 
  type: builtin                                                                                                                                                                                                                             
'545':                                                                                                                                                                                                                                      
  groupname: Users                                                                                                                                                                                                                          
  type: builtin                                                                                                                                                                                                                             
'546':                                                                                                                                                                                                                                      
  groupname: Guests                                                                                                                                                                                                                         
  type: builtin                                                                                                                                                                                                                             
'548':                                                                                                                                                                                                                                      
  groupname: Account Operators                                                                                                                                                                                                              
  type: builtin                                                                                                                                                                                                                             
'549':                                                                                                                                                                                                                                      
  groupname: Server Operators                                                                                                                                                                                                               
  type: builtin                                                                                                                                                                                                                             
'550':                                                                                                                                                                                                                                      
  groupname: Print Operators                                                                                                                                                                                                                
  type: builtin                                                                                                                                                                                                                             
'551':                                                                                                                                                                                                                                      
  groupname: Backup Operators                                                                                                                                                                                                               
  type: builtin                                                                                                                                                                                                                             
'552':                                                                                                                                                                                                                                      
  groupname: Replicator                                                                                                                                                                                                                     
  type: builtin                                                                                                                                                                                                                             
'553':                                                                                                                                                                                                                                      
  groupname: RAS and IAS Servers                                                                                                                                                                                                            
  type: local                                                                                                                                                                                                                               
'554':                                                                                                                                                                                                                                      
  groupname: Pre-Windows 2000 Compatible Access                                                                                                                                                                                             
  type: builtin                                                                                                                                                                                                                             
'555':                                                                                                                                                                                                                                      
  groupname: Remote Desktop Users                                                                                                                                                                                                           
  type: builtin                                                                                                                                                                                                                             
'556':                                                                                                                                                                                                                                      
  groupname: Network Configuration Operators                                                                                                                                                                                                
  type: builtin                                                                                                                                                                                                                             
'557':                                                                                                                                                                                                                                      
  groupname: Incoming Forest Trust Builders                                                                                                                                                                                                 
  type: builtin                                                                                                                                                                                                                             
'558':                                                                                                                                                                                                                                      
  groupname: Performance Monitor Users                                                                                                                                                                                                      
  type: builtin                                                                                                                                                                                                                             
'559':                                                                                                                                                                                                                                      
  groupname: Performance Log Users                                                                                                                                                                                                          
  type: builtin                                                                                                                                                                                                                             
'560':                                                                                                                                                                                                                                      
  groupname: Windows Authorization Access Group                                                                                                                                                                                             
  type: builtin                                                                                                                                                                                                                             
'561':                                                                                                                                                                                                                                      
  groupname: Terminal Server License Servers                                                                                                                                                                                                
  type: builtin                                                                                                                                                                                                                             
'562':                                                                                                                                                                                                                                      
  groupname: Distributed COM Users                                                                                                                                                                                                          
  type: builtin                                                                                                                                                                                                                             
'568':                                                                                                                                                                                                                                      
  groupname: IIS_IUSRS                                                                                                                                                                                                                      
  type: builtin                                                                                                                                                                                                                             
'569':                                                                                                                                                                                                                                      
  groupname: Cryptographic Operators                                                                                                                                                                                                        
  type: builtin                                                                                                                                                                                                                             
'571':                                                                                                                                                                                                                                      
  groupname: Allowed RODC Password Replication Group                                                                                                                                                                                        
  type: local                                                                                                                                                                                                                               
'572':                                                                                                                                                                                                                                      
  groupname: Denied RODC Password Replication Group                                                                                                                                                                                         
  type: local                                                                                                                                                                                                                               
'573':                                                                                                                                                                                                                                      
  groupname: Event Log Readers                                                                                                                                                                                                              
  type: builtin                                                                                                                                                                                                                             
'574':                                                                                                                                                                                                                                      
  groupname: Certificate Service DCOM Access                                                                                                                                                                                                
  type: builtin                                                                                                                                                                                                                             
'575':                                                                                                                                                                                                                                      
  groupname: RDS Remote Access Servers                                                                                                                                                                                                      
  type: builtin                                                                                                                                                                                                                             
'576':                                                                                                                                                                                                                                      
  groupname: RDS Endpoint Servers                                                                                                                                                                                                           
  type: builtin                                                                                                                                                                                                                             
'577':                                                                                                                                                                                                                                      
  groupname: RDS Management Servers                                                                                                                                                                                                         
  type: builtin                                                                                                                                                                                                                             
'578':                                                                                                                                                                                                                                      
  groupname: Hyper-V Administrators                                                                                                                                                                                                         
  type: builtin                                                                                                                                                                                                                             
'579':                                                                                                                                                                                                                                      
  groupname: Access Control Assistance Operators                                                                                                                                                                                            
  type: builtin                                                                                                                                                                                                                             
'580':                                                                                                                                                                                                                                      
  groupname: Remote Management Users                                                                                                                                                                                                        
  type: builtin                                                                                                                                                                                                                             
'582':                                                                                                                                                                                                                                      
  groupname: Storage Replica Administrators                                                                                                                                                                                                 
  type: builtin                                                                                                                                                                                                                             

 =======================================
|    Shares via RPC on resourced.pgp    |
 =======================================
[*] Enumerating shares
[+] Found 0 share(s) for user '' with password '', try a different user

 ==========================================
|    Policies via RPC for resourced.pgp    |
 ==========================================
[*] Trying port 445/tcp
[+] Found policy:
Domain password information:                                                                                                                                                                                                                
  Password history length: 24                                                                                                                                                                                                               
  Minimum password length: 7                                                                                                                                                                                                                
  Minimum password age: 1 day 4 minutes                                                                                                                                                                                                     
  Maximum password age: 41 days 23 hours 53 minutes                                                                                                                                                                                         
  Password properties:                                                                                                                                                                                                                      
  - DOMAIN_PASSWORD_COMPLEX: true                                                                                                                                                                                                           
  - DOMAIN_PASSWORD_NO_ANON_CHANGE: false                                                                                                                                                                                                   
  - DOMAIN_PASSWORD_NO_CLEAR_CHANGE: false                                                                                                                                                                                                  
  - DOMAIN_PASSWORD_LOCKOUT_ADMINS: false                                                                                                                                                                                                   
  - DOMAIN_PASSWORD_PASSWORD_STORE_CLEARTEXT: false                                                                                                                                                                                         
  - DOMAIN_PASSWORD_REFUSE_PASSWORD_CHANGE: false                                                                                                                                                                                           
Domain lockout information:                                                                                                                                                                                                                 
  Lockout observation window: 30 minutes                                                                                                                                                                                                    
  Lockout duration: 30 minutes                                                                                                                                                                                                              
  Lockout threshold: None                                                                                                                                                                                                                   
Domain logoff information:                                                                                                                                                                                                                  
  Force logoff time: not set                                                                                                                                                                                                                

 ==========================================
|    Printers via RPC for resourced.pgp    |
 ==========================================
[-] Could not get printer info via 'enumprinters': STATUS_ACCESS_DENIED

Completed after 18.18 seconds
