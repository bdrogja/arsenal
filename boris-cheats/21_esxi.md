# MENU

#plateform/linux #target/remote #cat/ESXI

% p-esxi, boris, vmware, esxi, vcenter, hypervisor, nmap, soap, version
## [ESXI-a] nmap - VMware/ESXi Version (SOAP /sdk Fingerprint)
```
nmap -Pn -p443 --open --script vmware-version <target_range>
```

% p-esxi, boris, esxi, port902, hypervisor, nmap, kandidaten
## [ESXI-b] nmap - ESXi Kandidaten (Port 902, VMware Auth Daemon)
```
nmap -Pn -sS -p902 --open <target_range> -oG - | awk '/902\/open/{print $2}'
```

% p-esxi, boris, vcenter, vami, port5480, nmap, kandidaten
## [ESXI-c] nmap - vCenter/VCSA Kandidaten (Port 5480 VAMI)
```
nmap -Pn -sS -p5480 --open <target_range> -oG - | awk '/5480\/open/{print $2}'
```
