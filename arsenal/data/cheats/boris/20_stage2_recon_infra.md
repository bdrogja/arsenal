# Boris S2 Recon & Infra

#plateform/linux #target/remote #cat/RECON

% boris, stage2, recon, dc, finddc, srv, dns, msdcs
## [S2.1a] Domain Controller finden (DNS SRV Record)
```
nslookup -type=SRV _ldap._tcp.dc._msdcs.<domain>
```

% boris, stage2, recon, dc, ldap, domain, nxc
## [S2.1b] nxc ldap - DC / Domain Info
```
nxc ldap <dc_ip> -u <user> -p <password>
```

% boris, stage2, recon, shares, spider, manspider, loot, creds, passwords
## [S2.2] manspider - Shares nach Passwoertern / Creds durchsuchen
```
manspider <target_range> -f passw user login cred kennwort secret -d <domain> -u <user> -p <password>
```

% boris, stage2, recon, vmware, esxi, vcenter, hypervisor, nmap, soap
## [S2.3a] nmap - VMware/ESXi Version (SOAP /sdk Fingerprint)
```
nmap -Pn -p443 --open --script vmware-version <target_range>
```

% boris, stage2, recon, esxi, port902, hypervisor, nmap
## [S2.3b] nmap - ESXi Kandidaten (Port 902, VMware Auth Daemon)
```
nmap -Pn -sS -p902 --open <target_range> -oG - | awk '/902\/open/{print $2}'
```

% boris, stage2, recon, vcenter, vami, port5480, nmap
## [S2.3c] nmap - vCenter/VCSA Kandidaten (Port 5480 VAMI)
```
nmap -Pn -sS -p5480 --open <target_range> -oG - | awk '/5480\/open/{print $2}'
```
