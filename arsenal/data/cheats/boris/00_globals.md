# START

#plateform/linux #target/local #cat/GLOBALS

% boris, globals, help, konvention, notation, reference, ntlm, kerberos, views, map
## Boris - Variablen-Konvention (LESEN, siehe auch: map)
domain = FQDN (corp.local) . user = sAMAccountName blank (jdoe, KEIN UPN, KEIN DOMAIN\) . password = roh (Quotes kommen erst im Kommando) . dc_ip = IP . dc_fqdn = DC FQDN (dc01.corp.local) . lhost = Angreifer-IP . target_range = CIDR
NTLM (Default, SMB/nxc/relay): IP-Target ok, domain egal. Kerberos (-k, certipy, nopac, LDAP): IMMER dc_fqdn + domain=FQDN, sonst KDC_ERR_WRONG_REALM / KRB_AP_ERR_MODIFIED.
certipy-ad braucht UPN: -u user@domain (FQDN). nxc/impacket/bloodhound: -u user blank + -d domain.
Binary auf Kali = certipy-ad (NICHT certipy). Zwei Sichten: Tool-Wort (certipy/nxc/...) ODER Weg-Wort (relay/adcs/coerce/rbcd/vulns/recon). Tippe map fuer die Uebersicht.
```
>show
```

% boris, globals, init, set, identity, start
## [INIT] Boris - Identity setzen (domain=FQDN / user=blank / password=roh / dc_ip)
```
>set domain=<domain> user=<user> password=<password> dc_ip=<dc_ip>
```

% boris, globals, init, set, network, dcfqdn, interface, lhost
## [INIT] Boris - DC-FQDN + Netz setzen (dc_fqdn / interface / lhost / target_range)
```
>set dc_fqdn=<dc_fqdn> interface=<interface> lhost=<lhost> target_range=<target_range>
```

% boris, globals, set, domain, fqdn
## Boris - domain setzen (FQDN, z.B. corp.local)
```
>set domain=<domain>
```

% boris, globals, set, user, samaccountname
## Boris - user setzen (blank, z.B. jdoe)
```
>set user=<user>
```

% boris, globals, set, password, roh
## Boris - password setzen (ROH, ohne Quotes)
```
>set password=<password>
```

% boris, globals, set, dcip, ip
## Boris - dc_ip setzen (IP)
```
>set dc_ip=<dc_ip>
```

% boris, globals, set, dcfqdn, fqdn, kerberos
## Boris - dc_fqdn setzen (FQDN, z.B. dc01.corp.local)
```
>set dc_fqdn=<dc_fqdn>
```

% boris, globals, show, list
## Boris - alle Globals anzeigen
```
>show
```

% boris, globals, clear, reset
## Boris - alle Globals loeschen
```
>clear
```
