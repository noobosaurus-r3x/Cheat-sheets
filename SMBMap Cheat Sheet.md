#smbmap
null password :
```bash
smbmap -u guest -p "" -d . -H 192.168.1.1
```

admin with password :
```bash
smbmap -u admin -p password123 -d . -H 192.168.1.1
```

executing a command : 
```bash
smbmap -u admin -p password123 -d . -H 192.168.1.1 -x 'ipconfig'
```

connect to specific drive :
```bash
smbmap -u admin -p password123 -d . -H 192.168.1.1 -r 'C$'
```

upload file :
```bash
smbmap -u admin -p password123 -d . -H 192.168.1.1 --upload '/path/to/file.txt' 'C$\file.txt'
```

download file :
```bash
smbmap -u admin -p password123 -d . -H 192.168.1.1 --download 'C$\file.txt'
```

enumerate a specific share :
```bash
smbmap -H 192.168.1.1 -s 'share name'
```

enumerate users :
```bash
smbmap -H 192.168.1.1 --users
```

`-R`  check shares with full permission
`-p` specify a port

