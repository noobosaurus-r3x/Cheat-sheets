#Metasploit #meterpreter
## Basic Commands

-   `help`: Display a list of available commands
-   `sysinfo`: Display system information, including OS and hostname
-   `ps`: List running processes
-   `kill`: Kill a process by PID
-   `migrate`: Migrate meterpreter to a new process
-   `rev2self`: Revert the current process to its original state

## File System Commands

-   `ls`: List files in the current directory
-   `cd`: Change the current directory
-   `pwd`: Display the current directory
-   `cat`: Display the contents of a file
-   `download`: Download a file from the target system
-   `upload`: Upload a file to the target system

## Network Commands

-   `ipconfig`: Display network configuration information
-   `route`: Display the routing table
-   `netstat`: Display active network connections
-   `portfwd`: Forward a local port to a remote service
-   `getsockname`: Display the socket name for a given connection

## User Management Commands

-   `getuid`: Display the user ID of the current user
-   `ps`: List running processes and their owner
-   `getprivs`: Display the privileges of the current user
-   `getsystem`: Attempt to escalate privileges to SYSTEM level

## Persistence Commands

-   `persistence`: Enable meterpreter persistence on the target system
-   `run`: Execute a script or command on startup

## Shell Commands

-   `shell`: Open a command prompt on the target system
-   `execute`: Execute a command on the target system
-   `background`: Background the current session
-   `Ctrl+Z`: Suspend the current session

## Other Commands

-   `use`: Load a meterpreter extension
-   `run`: Execute a script or command within an extension
-   `keyscan_start`: Start logging keystrokes on the target system
-   `keyscan_dump`: Dump the logged keystrokes
-   `screenshot`: Take a screenshot of the desktop on the target system
-   `webcam_list`: List available webcams on the target system
-   `webcam_snap`: Take a snapshot from a webcam on the target system
-   `hashdump`: Dump the password hashes from the target system
-   `timestomp`: Modify the timestamp of a file on the target system