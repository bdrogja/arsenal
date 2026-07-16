# SETUP

#plateform/linux #target/local #cat/SETUP

% p-setup, boris, globals, help, konvention, notation, reference, ntlm, kerberos
## Variablen-Konvention (LESEN; tippe map fuer die Seiten-Uebersicht)
domain = FQDN (corp.local) . user = sAMAccountName blank (jdoe, KEIN UPN, KEIN DOMAIN\) . password = roh (Quotes erst im Kommando) . dc_ip = IP . dc_fqdn = DC FQDN (dc01.corp.local) . lhost = Angreifer-IP . target_range = CIDR
NTLM (SMB/nxc/relay): IP-Target ok, domain egal. Kerberos (-k, certipy-ad, nopac, LDAP): IMMER dc_fqdn + domain=FQDN, sonst KDC_ERR_WRONG_REALM.
certipy-ad braucht UPN: -u user@domain (FQDN). nxc/impacket/bloodhound: -u user blank + -d domain. Binary = certipy-ad (NIE certipy); impacket = impacket-<tool> (NIE .py).
```
>show
```

% p-setup, boris, globals, init, set, identity, start
## [INIT] Identity setzen (domain=FQDN / user=blank / password=roh / dc_ip)
```
>set domain=<domain> user=<user> password=<password> dc_ip=<dc_ip>
```

% p-setup, boris, globals, init, set, network, dcfqdn, interface, lhost
## [INIT] DC-FQDN + Netz setzen (dc_fqdn / interface / lhost / target_range)
```
>set dc_fqdn=<dc_fqdn> interface=<interface> lhost=<lhost> target_range=<target_range>
```

% p-setup, boris, globals, set, domain, fqdn
## domain setzen (FQDN, z.B. corp.local)
```
>set domain=<domain>
```

% p-setup, boris, globals, set, user, samaccountname
## user setzen (blank, z.B. jdoe)
```
>set user=<user>
```

% p-setup, boris, globals, set, password, roh
## password setzen (ROH, ohne Quotes)
```
>set password=<password>
```

% p-setup, boris, globals, set, dcip, ip
## dc_ip setzen (IP)
```
>set dc_ip=<dc_ip>
```

% p-setup, boris, globals, set, dcfqdn, fqdn, kerberos
## dc_fqdn setzen (FQDN, z.B. dc01.corp.local)
```
>set dc_fqdn=<dc_fqdn>
```

% p-setup, boris, globals, show, list
## alle Globals anzeigen
```
>show
```

% p-setup, boris, globals, clear, reset
## alle Globals loeschen
```
>clear
```
