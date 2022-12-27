#nmap
# **Basic Syntax**
```bash
nmap <target IP>
nmap -sT -sS -Pn -v 10.10.10.10
sudo nmap -A -sS -Pn 10.10.10.10
sudo nmap -sV -sT -O -p- -vv --script vulners 10.10.10.10
```

# **Common flags**
```bash
-p <PORT> #Scan specific port
-p- #Scan all ports
-sT #TCP scan
-sU #UDP scan
-sV #Try to find services and versions running on the target
-Pn #Disable ping
-sS #Syn scan also called "Stealth Scan"
-oA #Output in all format
-sn #Host discovery (no port scan)
-A #Aggressive mode engaged
-O #Find operating system
-v -vv -vvv #Verbose mode
--script vuln
--script vulners
--script=http-enum Act a bit like nikto
```

# **Output formats**
```bash
-oN "Normal output"
-oX XML format
-oG Greppable format
-oA All 3 formats from above
```



# SMB Scripts


```bash
nmap -p445 --script smb-security-mode 192.168.1.1
```

Enumerate sessions :
```bash
nmap -p445 --script smb-enum-sessiom 192.168.1.1

nmap -p445 --script smb-enum-sessiom --script-args smbusername=administrator,smbpassword=password 192.168.1.1
```

Enumerate shares :
```bash
nmap -p445 --script smb-enum-shares 192.168.1.1

nmap -p445 --script smb-enum-shares --script-args smbusername=administrator,smbpassword=password 192.168.1.1
```

Enumerate shares and ls :
```bash
nmap -p445 --script smb-enum-shares,smb-ls --script-args smbusername=administrator,smbpassword=password 192.168.1.1
```

Enumerate users :
```bash
nmap -p445 --script smb-enum-users --script-args smbusername=administrator,smbpassword=password 192.168.1.1
```

Enumerate stats :
```bash
nmap -p445 --script smb-enum-stats --script-args smbusername=administrator,smbpassword=password 192.168.1.1
```

Enumerate domains :
```bash
nmap -p445 --script smb-enum-domains --script-args smbusername=administrator,smbpassword=password 192.168.1.1
```

Enumerates groups :
```bash
nmap -p445 --script smb-enum-groups --script-args smbusername=administrator,smbpassword=password 192.168.1.1
```


# SSH Scripts

```bash
#enumerate algorithms
nmap 192.168.1.1 -p 22 --script ssh2-enum-algos

#enumerate hostkeys
nmap 192.168.1.1 -p 22 --script ssh-hostkey --script-args ssh_hostkey=full

#enumerate auth. methods for admin user
nmap 192.168.1.1 -p 22 --script ssh-auth-methods --script-args="ssh.user=admin"

#bruteforce
hydra -l 'user' -P 'passwords_worldist' 192.168.1.1 ssh
```
