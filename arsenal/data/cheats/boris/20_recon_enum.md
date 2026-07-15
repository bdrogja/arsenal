# WEG

#plateform/linux #target/remote #cat/RECON

% boris, recon, enum, dc, finddc, srv, dns, msdcs
## [RECON-a] Domain Controller finden (DNS SRV Record)
```
nslookup -type=SRV _ldap._tcp.dc._msdcs.<domain>
```

% boris, recon, enum, dc, ldap, domain, nxc, info
## [RECON-b] nxc ldap - DC / Domain Info
```
nxc ldap <dc_ip> -u <user> -p '<password>' -d <domain>
```

% boris, recon, enum, smb, nullsession, shares, passpol, users, nxc
## [RECON-c] nxc - Null-Session Enum (shares / users / pass-pol)
```
nxc smb <dc_ip> -u '' -p '' --shares --users --pass-pol
```

% boris, recon, enum, ridbrute, users, nxc, unauth
## [RECON-d] nxc - RID Brute (User-Enum ohne Creds)
```
nxc smb <dc_ip> -u '' -p '' --rid-brute 5000
```

% boris, recon, enum, spray, useraspassword, nxc, spraying
## [RECON-e] nxc - Username = Password Spray (u/p = Datei, kein Quoting)
```
nxc smb <target_range> -u <users_file|users.txt> -p <users_file|users.txt> --no-bruteforce --continue-on-success
```

% boris, recon, enum, shares, spider, manspider, loot, creds, passwords, kennwort
## [RECON-f] manspider - Shares nach Passwoertern / Creds durchsuchen
```
manspider <target_range> -f passw user login cred kennwort secret -d <domain> -u <user> -p '<password>'
```

% boris, recon, enum, vmware, esxi, vcenter, hypervisor, nmap, soap
## [RECON-g] nmap - VMware/ESXi Version (SOAP /sdk Fingerprint)
```
nmap -Pn -p443 --open --script vmware-version <target_range>
```

% boris, recon, enum, esxi, port902, hypervisor, nmap
## [RECON-h] nmap - ESXi Kandidaten (Port 902, VMware Auth Daemon)
```
nmap -Pn -sS -p902 --open <target_range> -oG - | awk '/902\/open/{print $2}'
```

% boris, recon, enum, vcenter, vami, port5480, nmap
## [RECON-i] nmap - vCenter/VCSA Kandidaten (Port 5480 VAMI)
```
nmap -Pn -sS -p5480 --open <target_range> -oG - | awk '/5480\/open/{print $2}'
```
