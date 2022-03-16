# **Basic Syntax**
`nmap <target IP>`

# **Common flags**

`-p <PORT>` Scan specific port

`-p-` Scan all ports

`-sT` TCP scan

`-sU` UDP scan

`-sV` Try to find services and versions running on the target

`-Pn` Disable ping

`-sS` Syn scan also called "Stealth Scan"

`-oA` Output in all format

`-sn` Host discovery (no port scan)

`-A` Aggressive mode engaged

`-O` Find operating system

`-v` `-vv` `-vvv` Verbose mode

# **Output formats**

`-oN` "Normal output"

`-oX` XML format

`-oG` Greppable format

`-oA` All 3 formats from above

# **Basic Scripts**

`--script vuln`

`-sV --script vulners`

`-sV --script=hhtp-enum` Act a bit like nikto 

# **Examples of scanning IP : 10.10.10.10**

`nmap -sT -sS -Pn -v 10.10.10.10`

`sudo nmap -A -sS -Pn 10.10.10.10`

`sudo nmap -sV -sT -O -p- -vv --script vulners 10.10.10.10`