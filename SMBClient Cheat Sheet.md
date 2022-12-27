#smbclient
## Basic usage

To connect to a file share using `smbclient`, use the following syntax:

Copy code
```bash
smbclient //server/share [options]
```

Replace `server` with the name or IP address of the server hosting the file share, and `share` with the name of the file share.

You will be prompted for your username and password for the file share. Once authenticated, you will be presented with a command prompt where you can enter various commands to interact with the file share.

## Options

Here are some of the options you can use with `smbclient`:

-   `-U`: Specify a username to use when connecting to the file share.
-   `-W`: Specify the domain or workgroup that the server belongs to.
-   `-I`: Specify the IP address of the server.
-   `-p`: Specify the port number to use for the connection.
-   `-d`: Set the debug level.
-   `-N`: Do not ask for a password when connecting to the file share.

## Available commands

Here are some of the most common commands you can use with `smbclient`:

-   `ls`: List the files and directories in the current directory.
-   `cd`: Change the current directory.
-   `put`: Upload a file to the file share.
-   `get`: Download a file from the file share.
-   `mput`: Upload multiple files to the file share.
-   `mget`: Download multiple files from the file share.
-   `rm`: Delete a file from the file share.
-   `mkdir`: Create a new directory on the file share.
-   `rmdir`: Delete a directory from the file share.
-   `pwd`: Print the current working directory.
-   `exit`: Disconnect from the file share and exit `smbclient`.

## Examples

Here are some examples of using `smbclient` to connect to and interact with a file share:

Copy code
```bash
# Connect to a file share and list the files and directories in the current directory 
smbclient //server/share ls  

# Connect to a file share using a specific username and password 
smbclient //server/share -U username%password  

# Connect to a file share using a specific IP address and port number 
smbclient //server/share -I 192.168.1.100 -p 139  

# Upload a file to the file share 
smbclient //server/share put /path/to/local/file  

# Download a file from the file share 
smbclient //server/share get /path/to/remote/file  

# Create a new directory on the file share 
smbclient //server/share mkdir newdirectory
```
