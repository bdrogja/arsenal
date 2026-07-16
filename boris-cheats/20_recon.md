# MENU

#plateform/linux #target/remote #cat/RECON

% p-recon, boris, enum, dc, finddc, srv, dns, msdcs
## [RECON-a] Domain Controller finden (DNS SRV Record)
```
nslookup -type=SRV _ldap._tcp.dc._msdcs.<domain>
```

% p-recon, boris, enum, dc, ldap, domain, nxc, info
## [RECON-b] nxc ldap - DC / Domain Info
```
nxc ldap <dc_ip> -u <user> -p '<password>' -d <domain>
```

% p-recon, boris, enum, smb, nullsession, shares, passpol, users, nxc
## [RECON-c] nxc - Null-Session Enum (shares / users / pass-pol)
```
nxc smb <dc_ip> -u '' -p '' --shares --users --pass-pol
```

% p-recon, boris, enum, ridbrute, users, nxc, unauth
## [RECON-d] nxc - RID Brute (User-Enum ohne Creds)
```
nxc smb <dc_ip> -u '' -p '' --rid-brute 5000
```

% p-recon, boris, enum, spray, useraspassword, nxc, spraying
## [RECON-e] nxc - Username = Password Spray (u/p = Datei, kein Quoting)
```
nxc smb <target_range> -u <users_file|users.txt> -p <users_file|users.txt> --no-bruteforce --continue-on-success
```

% p-recon, boris, enum, spray, passwordspray, singlepassword, nxc, spraying
## [RECON-f] nxc - Ein Passwort gegen User-Liste (Password Spray)
Ein Kennwort ueber viele User. Lockout-Policy beachten (RECON-c pass-pol)!
```
nxc smb <target_range> -u <users_file|users.txt> -p '<password>' -d <domain> --continue-on-success
```

% p-recon, boris, enum, bloodhound, collector, graph, python, firstmove
## [RECON-g] bloodhound-python - Domain graphen (Kern-Frueh-Schritt)
Allgemeine Collection (nicht nur ADCS). -dc dc_fqdn=FQDN . -ns dc_ip=IP . -u blank.
```
bloodhound-python -u <user> -p '<password>' -d <domain> -c all -dc <dc_fqdn> -ns <dc_ip> --zip
```

% p-recon, boris, enum, shares, spider, manspider, loot, passwords, kennwort
## [RECON-h] manspider - Shares nach Passwoertern / Creds durchsuchen
```
manspider <target_range> -f passw user login cred kennwort secret -d <domain> -u <user> -p '<password>'
```
