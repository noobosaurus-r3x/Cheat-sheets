#ffuf
# Basic Syntax
```bash
ffuf -c -w path/to/wordlist -u https://target_url/FUZZ
```

```bash
#Filter any response with a content size of 4242 bytes
ffuf -w /path/to/vhost/wordlist -uhttps://target_url/ -H "Host: FUZZ" -fs 4242

#Filter any response with a 401 status response
ffuf -w /path/to/values.txt -u https://target_url/script.php?valid_name=FUZZ -fc 401

#Filter 401 status and fuzz the password in POST request
ffuf -w /path/to/postdata.txt -X POST -d "username=admin\&password=FUZZ" -u https://target_url/login.php -fc 401
```

# Common flags
-   `-c`: Colorize the output
-   `-maxtime`: Maximum time in seconds for the process
-   `-p`: Delay in seconds between requests (e.g. `0.1`)
-   `-v`: Verbose output
-   `-t`: Number of threads (default is 40)
-   `-mc`: Match HTTP status code (e.g. `200`, `204`, `301`, `302`, `307`, `401`, `403`, `405`, `500` or `all`)
-   `-fc`: Filter out codes
-   `-w`: Path to wordlist
-   `-u`: Target URL
-   `-s`: Silent mode
-   `-recursion`: Enable recursion
-  `-r`: Follow redirects
-   `-o`: Output results to a file
-   `-of`: Output format (e.g. `json`, `ejson`, `html`, `md`, `csv`, `ecsv` or `all`)
-   `-b`: Cookies to include in requests


# Some Examples

```bash
#Match all response, filter 42 bytes answers, output coloured and verbose.
ffuf -w wordlist.txt -u https://example.org/FUZZ -mc all -fs 42 -c -v

#Fuzz host header and show only status 200
ffuf -w hosts.txt -u https://example.org/ -H "Host: FUZZ" -mc 200

#Fuzz the `name` field in a POST request with JSON data, filter out responses containing the text "error".
ffuf -w entries.txt -u https://example.org/ -X POST -H "Content-Type: application/json" \
-d '{"name": "FUZZ", "anotherkey": "anothervalue"}' -fr "error"

#Use 2 wordlists for the VAL field. Match responses containg the VAL keyword and will be coloured
ffuf -w params.txt:PARAM -w values.txt:VAL -u https://example.org/?PARAM=VAL -mr "VAL" -c
```


# Other Tips

- FFUF has an **interactive mode** that can be accessed by pressing `Enter` while the tool is running. This will drop a shell with the ability to run commands such as reconfiguring filters, saving the current state, and more.
-   FFUF supports multiple payload locations in the same request by using the `FUZZ` keyword multiple times. For example: `https://example.org/path/FUZZ/another_path/FUZZ`
-   FFUF also supports using variables to specify payload locations. For example: `https://example.org/path/{var1}/another_path/{var2}`
