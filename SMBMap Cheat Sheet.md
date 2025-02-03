# SMBMap Cheat Sheet  

## **1. Basic Terminology**  
- **SMBMap**: A tool to enumerate SMB shares, permissions, and perform file operations across Windows domains.  
- **SMB (Server Message Block)**: Protocol for shared access to files, printers, and ports.  
- **Share**: A network folder or resource exposed over SMB (e.g., `C$`, `ADMIN$`).  
- **Null Session**: Connecting to SMB without credentials (often restricted in modern systems).  

---
## **2. Basic Usage**  

### General Syntax  
```bash  
smbmap [options] -H <IP>  
```  

### Common Flags  
| **Flag**             | **Description**                                                                 |  
|----------------------|---------------------------------------------------------------------------------|  
| `-H <IP>`            | Target IP address                                                              |  
| `-u <user>`          | Username (use `-u ''` for null session)                                         |  
| `-p <password>`      | Password                                                                        |  
| `-d <domain>`        | Domain/workgroup (use `.` for local workgroup)                                  |  
| `-s <share>`         | Specific share to target                                                        |  
| `-P <port>`          | Custom SMB port (default: 445)                                                  |  
| `-x <command>`       | Execute a command on the target                                                |  
| `-R`                 | Recursive directory listing (with read permissions)                             |  
| `--download <file>`  | Download a file from the share                                                  |  
| `--upload <src> <dst>` | Upload a file to the share                                                     |  
| `--users`            | Enumerate users                                                                 |  
| `--admin`            | Check if user has admin access                                                  |  
| `-v`                 | Verbose output                                                                  |  

---

## **3. Practical Examples**  

### **Authentication**  
**Null Session (Guest Access):**  
```bash  
smbmap -u '' -p '' -d . -H 192.168.1.1  
```  

**Authenticated Access:**  
```bash  
smbmap -u admin -p 'Password123!' -d WORKGROUP -H 192.168.1.1  
```  

---

### **Share Enumeration**  
**List All Shares:**  
```bash  
smbmap -H 192.168.1.1  
```  

**Check Specific Share:**  
```bash  
smbmap -H 192.168.1.1 -s 'C$'  
```  

**Check Shares with Full Permissions:**  
```bash  
smbmap -H 192.168.1.1 -R  
```  

---

### **File Operations**  
**Download a File:**  
```bash  
smbmap -u admin -p 'Password123!' -H 192.168.1.1 --download 'C$\secret.txt'  
```  

**Upload a File:**  
```bash  
smbmap -u admin -p 'Password123!' -H 192.168.1.1 --upload '/tmp/payload.exe' 'C$\Windows\Temp\payload.exe'  
```  

**Recursive Directory Listing:**  
```bash  
smbmap -u guest -p '' -H 192.168.1.1 -R 'Documents'  
```  

---

### **Command Execution**  
**Run a Command (e.g., `whoami`):**  
```bash  
smbmap -u admin -p 'Password123!' -H 192.168.1.1 -x 'whoami'  
```  

**Execute a PowerShell Script:**  
```bash  
smbmap -u admin -p 'Password123!' -H 192.168.1.1 -x 'powershell -c "Get-Process"'  
```  

---

### **User & Permission Checks**  
**Enumerate Users:**  
```bash  
smbmap -H 192.168.1.1 --users  
```  

**Check Admin Access:**  
```bash  
smbmap -u admin -p 'Password123!' -H 192.168.1.1 --admin  
```  

---

## **4. Advanced Techniques**  

### **Port Redirection**  
Target non-standard SMB port (e.g., 8445):  
```bash  
smbmap -H 192.168.1.1 -P 8445  
```  

### **Read-Only Checks**  
Check if shares are writable:  
```bash  
smbmap -u guest -p '' -H 192.168.1.1 --no-write-check  
```  

### **Brute-Force Share Names**  
Combine with `enum4linux` or `nmap` scripts for share discovery.  

---

## **5. Useful Tips**  
- Use `-v` for debugging connection issues.  
- Combine with `crackmapexec` for lateral movement.  
- For interactive sessions, use `smbclient` (e.g., `smbclient //192.168.1.1/C$ -U admin`).  
- Always check permissions (`--admin`, `-R`) before attempting writes.  

---

## **6. References**  
- [SMBMap GitHub](https://github.com/ShawnDEvans/smbmap)  
