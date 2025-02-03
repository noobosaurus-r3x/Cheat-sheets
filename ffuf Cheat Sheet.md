# FFUF Cheat Sheet

## **1. Basic Syntax**

```bash
ffuf -c -w /path/to/wordlist -u https://target/FUZZ
```

- **`-c`**: Colorize output
- **`-w`**: Path to wordlist
- **`-u`**: Target URL (use `FUZZ` for placeholder)

---

## **2. Common Flags**

|**Flag**|**Description**|
|---|---|
|`-t <threads>`|Number of threads (default 40)|
|`-mc`|Match response codes (e.g., `200`, `403`, `all`)|
|`-fc`|Filter out response codes (e.g., `-fc 401`)|
|`-fs <size>`|Filter by response size in bytes|
|`-fw <count>`|Filter by the number of words|
|`-fl <count>`|Filter by the number of lines|
|`-p <secs>`|Delay between requests (e.g., `-p 0.1`)|
|`-maxtime`|Maximum total scan time in seconds|
|`-s`|Silent mode|
|`-v`|Verbose mode|
|`-H <header>`|Add custom header (e.g., `-H "Host: FUZZ"`)|
|`-r`|Follow redirects|
|`-recursion`|Enable recursion|
|`-b <cookie>`|Cookies (e.g., `-b "PHPSESSID=XYZ"`)|
|`-o <file>`|Output results to a file|
|`-of <fmt>`|Output format (`json`, `html`, `md`, `csv`, etc.)|
|`-fr <regex>`|Filter out responses containing a regex (e.g., `-fr "error|
|`-mr <regex>`|Match responses containing a regex (inverse of `-fr`)|
|`-ic`|Ignore worlists comments|

---

## **3. Usage Examples**

### 3.1 **Filtering by Status/Size**

```bash
# Filter responses of 4242 bytes using the Host header
ffuf -w /path/vhosts.txt \
     -u https://target/ \
     -H "Host: FUZZ" \
     -fs 4242
```

```bash
# Filter out 401 status
ffuf -w /path/values.txt \
     -u https://target/script.php?valid_name=FUZZ \
     -fc 401
```

### 3.2 **POST Request Fuzzing**

```bash
# Fuzz password, filter out 401
ffuf -w /path/postdata.txt \
     -X POST \
     -d "username=admin&password=FUZZ" \
     -u https://target/login.php \
     -fc 401
```

### 3.3 **Match/Filter by Patterns**

```bash
# Match all responses, filter out 42-byte answers, color, verbose
ffuf -w wordlist.txt \
     -u https://example.org/FUZZ \
     -mc all \
     -fs 42 \
     -c -v
```

```bash
# Fuzz Host header, show only 200
ffuf -w hosts.txt \
     -u https://example.org/ \
     -H "Host: FUZZ" \
     -mc 200
```

```bash
# Fuzz JSON body, filter out responses containing "error"
ffuf -w entries.txt \
     -u https://example.org/ \
     -X POST \
     -H "Content-Type: application/json" \
     -d '{"name": "FUZZ", "another": "value"}' \
     -fr "error"
```

### 3.4 **Multiple FUZZ Positions**

```bash
# 2 wordlists, replace PARAM and VAL, match any response containing "VAL"
ffuf -w params.txt:PARAM -w values.txt:VAL \
     -u https://example.org/?PARAM=VAL \
     -mr "VAL" \
     -c
```

---

## **4. Recursion and Advanced Options**

```bash
# Recursive scan, scanning discovered directories
ffuf -w wordlist.txt \
     -u https://example.org/FUZZ \
     -recursion \
     -mc 200
```

- **`-recursion-depth <N>`**: Control how many levels to recurse.
- **`-recursion-strategy greedy|default`**: Adjust how recursion behaves.
- **`-mode clusterbomb`**: Multi-wordlist advanced fuzzing strategy.

---

## **5. Example of Wordlists (SecLists)**

SecLists provides a large variety of wordlists:

- **Directory/Endpoints**:
    
    - `Discovery/Web-Content/common.txt`
    - `Discovery/Web-Content/directory-list-2.3-small.txt`
    - `Discovery/Web-Content/raft-medium-directories-lowercase.txt`
- **Subdomains**:
    
    - `Discovery/DNS/fierce-hostlist.txt`
    - `Discovery/DNS/subdomains-top1million-110000.txt`
- **Parameters**:
    
    - `Discovery/Web-Content/burp-parameter-names.txt`
    - `Fuzzing/parameter_names.txt`
- **VHosts / Virtual Hosts**:
    
    - `Discovery/DNS/vhosts.txt`
    - `Discovery/DNS/virtualhosts.txt`
- **Passwords / Usernames**:
    
    - `Passwords/Common-Credentials/10k-most-common.txt`
    - `Usernames/top-usernames-shortlist.txt`

**Pro Tip**: Clone [SecLists](https://github.com/danielmiessler/SecLists) and reference these lists directly in your ffuf commands:

```bash
git clone https://github.com/danielmiessler/SecLists.git
```

Then specify paths like `-w SecLists/Discovery/Web-Content/common.txt`.

---

## **6. Interactive Mode**

- Press **Enter** while ffuf is running to enter interactive mode.
- **`filters`**: Show current filters
- **`matchers`**: Show current matchers
- **`save json /path/out.json`**: Save current results in JSON

---

## **References**

- [FFUF GitHub](https://github.com/ffuf/ffuf)
- [FFUF Wiki](https://github.com/ffuf/ffuf/wiki)
- [SecLists GitHub](https://github.com/danielmiessler/SecLists)

