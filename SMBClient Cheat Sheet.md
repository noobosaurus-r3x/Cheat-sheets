# smbclient Cheat Sheet  

## **1. Basic Terminology**  
- **SMB (Server Message Block)**: Protocol for file sharing, printers, and network communication.  
- **smbclient**: Command-line tool to interact with SMB shares (similar to FTP).  
- **Share**: A network folder or resource exposed via SMB (e.g., `C$`, `Documents`).  
- **Workgroup/Domain**: Network group for shared resources (default: `WORKGROUP`).  

---

## 2. Basic Usage**  

### General Syntax  
```bash  
smbclient [options] //<server>/<share>  
```  
Replace `server` with the name or IP address of the server hosting the file share, and `share` with the name of the file share.

You will be prompted for your username and password for the file share. Once authenticated, you will be presented with a command prompt where you can enter various commands to interact with the file share.
### Common Options  
| **Option**          | **Description**                                                                 |  
|---------------------|---------------------------------------------------------------------------------|  
| `-U <user>[%pass]`  | Username and password (e.g., `-U admin%Password123`).                           |  
| `-W <domain>`       | Workgroup/domain name (default: `WORKGROUP`).                                   |  
| `-I <IP>`           | Server IP address (bypasses DNS).                                               |  
| `-p <port>`         | Custom port (default: 445 for SMB over TCP/IP).                                 |  
| `-N`                | No password prompt (use with empty or guest access).                            |  
| `-E`                | Hide password prompt output.                                                    |  
| `-c <command>`      | Execute a command non-interactively (e.g., `-c 'ls'`).                          |  
| `-A <creds-file>`   | Load credentials from a file (format: `username = admin`, `password = pass`).   |  
| `-m <max-protocol>` | Set max SMB protocol (e.g., `-m SMB3`).                                         |  
| `-d <debug-level>`  | Debug verbosity (0-10).                                                         |  

---

## **3. Interactive Commands**  

Once connected, use these commands:

|**Command**|**Description**|
|---|---|
|`ls`|List files/directories.|
|`cd <dir>`|Change directory.|
|`get <file>`|Download a file.|
|`put <file>`|Upload a file.|
|`mget <pattern>`|Download multiple files (e.g., `mget *.txt`).|
|`mput <pattern>`|Upload multiple files.|
|`rm <file>`|Delete a file.|
|`mkdir <dir>`|Create a directory.|
|`rmdir <dir>`|Delete a directory.|
|`pwd`|Print current directory.|
|`recurse`|Toggle recursive mode for `mget`/`mput`.|
|`mask <filter>`|Set a file filter (e.g., `mask *.docx`).|
|`tar`|Create/extract tar backups (e.g., `tar c backup.tar *`).|
|`exit`|Quit smbclient.|

---

## **4. Practical Examples**  

### **Connection**  
**Anonymous/Guest Access:**  
```bash  
smbclient //192.168.1.10/public -N  
```  

**Authenticated Access:**  
```bash  
smbclient //192.168.1.10/C$ -U admin%Password123  
```  

**Specify Domain/Workgroup:**  
```bash  
smbclient //SERVER/Share -W CORP -U user%pass  
```  

---

### **File Operations**  
**Download a File:**  
```bash  
smbclient //192.168.1.10/Data -U user%pass -c "get report.docx"  
```  

**Upload All TXT Files:**  
```bash  
smbclient //192.168.1.10/Data -U user%pass -c "mask *.txt; recurse; mput *.txt"  
```  

**Delete a File:**  
```bash  
smbclient //192.168.1.10/Data -U user%pass -c "rm oldfile.zip"  
```  

---

### **Directory Management**  
**Create a Directory:**  
```bash  
smbclient //192.168.1.10/Data -U user%pass -c "mkdir Projects"  
```  

**Recursive Download:**  
```bash  
smbclient //192.168.1.10/Data -U user%pass -c "recurse; prompt; mget *"  
```  

---

### **Non-Interactive Mode**  
**List Shares via Script:**  
```bash  
smbclient -L 192.168.1.10 -U admin%Password123 -N -I 192.168.1.10  
```  

**Backup Directory to Tar:**  
```bash  
smbclient //192.168.1.10/Backup -U user%pass -c "tar c backup.tar Documents"  
```  

---

## **5. Advanced Techniques**  

### **Mounting Shares**  
Use `mount.cifs` (Linux) or `net use` (Windows) for persistent access:  
```bash  
sudo mount -t cifs //192.168.1.10/Data /mnt/share -o user=admin,pass=Password123  
```  

### **Brute-Force Share Names**  
Combine with tools like `nmap` or `enum4linux`:  
```bash  
enum4linux -S 192.168.1.10  
```  

### **Using a Credentials File**  
Create `creds.txt`:  
```ini  
username = admin  
password = Password123  
domain = CORP  
```  
Then:  
```bash  
smbclient //192.168.1.10/C$ -A creds.txt  
```  

---

## **6. Troubleshooting**  

- **Connection Refused**: Check firewall rules, SMB port (445/139), and service status.  
- **Access Denied**: Verify credentials, share permissions, and user privileges.  
- **Protocol Errors**: Use `-m SMB2` or `-m SMB3` to enforce protocol version.  

---

## **7. References**  
- [smbclient Man Page](https://www.samba.org/samba/docs/current/man-html/smbclient.1.html)  
