# Tshark Cheat Sheet  
*Command-Line Network Protocol Analyzer*

---

## Overview

- **Tshark**: A tool for capturing and analyzing network traffic via the command line.
- **Capture Filters**: (BPF syntax) Applied during capture to limit the data saved.
- **Display Filters**: (Wireshark filtering language) Applied when reading a capture file.
- **Use Cases**: Live monitoring, offline analysis, protocol troubleshooting, and generating statistics.

---

## Installation

### Ubuntu/Debian
```bash
sudo apt-get install tshark
```

### Arch Linux
```bash
sudo pacman -S tshark
```

---

## Common Flags and Options

| **Flag**                       | **Description**                                                                                      | **Example**                                   |
|--------------------------------|------------------------------------------------------------------------------------------------------|-----------------------------------------------|
| `-i <interface>`               | Specify the network interface (e.g., `eth0`, `wlan0`) to capture packets from.                        | `sudo tshark -i eth0`                         |
| `-w <file>`                    | Write captured packets to a file in PCAP format.                                                     | `sudo tshark -i eth0 -w capture.pcap`           |
| `-r <file>`                    | Read packets from a saved capture file.                                                              | `tshark -r capture.pcap`                        |
| `-f "<capture filter>"`        | Apply a capture filter (BPF syntax) during capture.                                                  | `sudo tshark -i eth0 -f "tcp port 80"`          |
| `-Y "<display filter>"`        | Apply a display filter to show specific packets from a capture file.                                  | `tshark -r capture.pcap -Y "http"`              |
| `-T <format>`                  | Specify output format (e.g., `fields`, `json`, `pdml`, `text`).                                        | `tshark -r capture.pcap -T json`                |
| `-e <field>`                   | Print a specific field in the output (used with `-T fields`).                                        | `-e http.request.method`                        |
| `-V`                           | Enable verbose output for detailed packet information.                                               | `sudo tshark -i eth0 -V`                        |
| `-c <count>`                   | Stop capture after a fixed number of packets.                                                        | `sudo tshark -i eth0 -c 100`                     |
| `-n`                           | Disable name resolution for faster capture and output (IP addresses remain numeric).                   | `sudo tshark -i eth0 -n`                        |
| `-q`                           | Quiet mode: minimal output, useful when combined with statistics or follow options.                   | `tshark -r capture.pcap -qz conv,ip`             |
| `-b <option>`                  | Enable ring buffer capture (split output files by size, duration, or packet count).                    | `sudo tshark -i eth0 -b filesize:102400 -b files:5`|
| `-o <preference:value>`        | Set configuration preferences on the command line.                                                 | `sudo tshark -i eth0 -o tcp.desegment_tcp_streams:TRUE`|
| `-z <statistic>`               | Generate statistics (e.g., I/O, conversations, endpoints).                                           | `tshark -r capture.pcap -z io,stat,5`            |


---

## Capture Filters (BPF Syntax)

Capture filters limit which packets are written to disk. They are applied during the capture process and use Berkeley Packet Filter (BPF) syntax.

| **Filter**            | **Description**                                           | **Example Usage**                                |
|-----------------------|-----------------------------------------------------------|--------------------------------------------------|
| `host <IP>`           | Capture packets to/from a specific IP address.          | `sudo tshark -i eth0 -f "host 192.168.1.1"`         |
| `port <number>`       | Capture packets using a specific port.                  | `sudo tshark -i eth0 -f "port 443"`                |
| `tcp` or `udp`        | Capture packets of the TCP or UDP protocol.             | `sudo tshark -i eth0 -f "tcp"`                     |
| `src <IP>` or `dst <IP>`| Capture packets from a specific source or destination IP. | `sudo tshark -i eth0 -f "src 10.0.0.1"`             |
| `not <protocol>`      | Exclude a specific protocol (e.g., ARP).                  | `sudo tshark -i eth0 -f "not arp"`                 |


---

## Display Filters

Display filters refine the view of a capture file. They use Wireshark’s filtering syntax, which is more powerful and flexible than BPF.

| **Filter**                        | **Description**                                             | **Example Usage**                                      |
|-----------------------------------|-------------------------------------------------------------|--------------------------------------------------------|
| `ip.addr == <IP>`                 | Show packets where either source or destination matches.    | `tshark -r capture.pcap -Y "ip.addr == 192.168.1.1"`      |
| `tcp.port == <number>`            | Show packets with a specific TCP port.                      | `tshark -r capture.pcap -Y "tcp.port == 443"`             |
| `http`                            | Display HTTP protocol packets.                              | `tshark -r capture.pcap -Y "http"`                        |
| `dns`                             | Display DNS queries and responses.                          | `tshark -r capture.pcap -Y "dns"`                         |
| `frame contains "<text>"`         | Display packets that contain a specific string.             | `tshark -r capture.pcap -Y 'frame contains "GET"'`         |


---

## Advanced Options & Techniques

### Ring Buffer Capture
Capture continuously while splitting output into multiple files.  
```bash
sudo tshark -i eth0 -b filesize:102400 -b files:5 -w capture.pcap
```
- *Explanation*: Each file is limited to 100MB; a maximum of 5 files are kept.

### TLS Decryption
Capture and decrypt TLS sessions using a key log file.  
```bash
sudo tshark -i eth0 -o tls.keylog_file:tls_keys.log -w tls_capture.pcap
```
- *Explanation*: Use a key log file (generated by your browser or application) to déchiffrer TLS traffic.

### Following a TCP Stream
Extract and view the full conversation for a TCP session.
```bash
tshark -r capture.pcap -qz follow,tcp,ascii,0
```
- *Explanation*: Replace `0` with the stream index to follow a specific TCP conversation.

### Field Extraction
Output specific protocol fields for further processing.
```bash
tshark -r capture.pcap -T fields -e ip.src -e ip.dst -e tcp.port
```
- *Explanation*: Use `-T fields` with `-e` options to display selected fields.

### Generating Statistics
Gather I/O statistics or conversation summaries.
- **I/O Statistics**:  
  ```bash
  tshark -r capture.pcap -z io,stat,5
  ```
  *Explanation*: Provides stats in 5-second intervals.
- **Conversation Summary**:  
  ```bash
  tshark -r capture.pcap -qz conv,ip
  ```
  *Explanation*: Summarizes IP conversations between hosts.


---

## Practical Examples


**HTTP and TLS Traffic**

- **Capture TCP Traffic on Port 80**  
  ```bash
  sudo tshark -i eth0 -f "tcp port 80" -w http_traffic.pcap
  ```  
  *Captures HTTP traffic on port 80.*

- **Capture HTTPS Traffic with TLS Decryption**  
  ```bash
  sudo tshark -i eth0 -o tls.keylog_file:tls_keys.log -Y "tls" -w https_traffic.pcap
  ```  
  *Uses a TLS key log file to déchiffrer HTTPS traffic.*

- **Analyze HTTP Requests and Extract Fields**  
  ```bash
  tshark -r capture.pcap -Y "http.request" -T fields -e http.request.method -e http.request.uri -e http.host
  ```  
  *Extracts request methods, URIs, and host headers from HTTP traffic.*

- **Extract HTTP Host Header**  
  ```bash
  tshark -r capture.pcap -Y "http.request" -T fields -e http.host
  ```  

---

**DNS Traffic**

- **Capture DNS Traffic**  
  ```bash
  sudo tshark -i eth0 -f "udp port 53" -w dns_traffic.pcap
  ```  

- **Analyze DNS Responses**  
  ```bash
  tshark -r capture.pcap -Y "dns.flags.response == 1" -T fields -e dns.qry.name
  ```  
  *Extracts queried domain names from DNS responses.*

---

**VoIP and Multimedia Traffic**

- **Capture and Analyze VoIP Traffic (SIP)**  
  ```bash
  sudo tshark -i eth0 -f "sip" -w voip_traffic.pcap
  ```  
  *Captures SIP traffic for VoIP call analysis.*

- **Capture Multicast Traffic**  
  ```bash
  sudo tshark -i eth0 -f "multicast" -w multicast_traffic.pcap
  ```  

---

**Network Monitoring and Troubleshooting**

- **Monitor ARP Traffic**  
  ```bash
  sudo tshark -i eth0 -f "arp" -w arp_traffic.pcap
  ```  

- **Capture ICMP Traffic**  
  ```bash
  sudo tshark -i eth0 -f "icmp" -w icmp_traffic.pcap
  ```  

- **Analyze TCP Retransmissions**  
  ```bash
  tshark -r capture.pcap -Y "tcp.analysis.retransmission"
  ```  

---

**Other Protocols**

- **Analyze FTP Traffic**  
  ```bash
  sudo tshark -i eth0 -f "ftp" -w ftp_traffic.pcap
  ```  

- **Capture and Analyze SMTP Traffic**  
  ```bash
  sudo tshark -i eth0 -f "smtp" -w smtp_traffic.pcap
  ```  

- **Capture and Analyze DHCP Traffic**  
  ```bash
  sudo tshark -i eth0 -f "dhcp" -w dhcp_traffic.pcap
  ```  

- **Capture and Analyze NTP Traffic**  
  ```bash
  sudo tshark -i eth0 -f "ntp" -w ntp_traffic.pcap
  ```  

- **Analyze SMB Traffic**  
  ```bash
  sudo tshark -i eth0 -f "smb" -w smb_traffic.pcap
  ```  

---

**Advanced Output and Filtering Options**

- **Disable DNS Resolution for Faster Capture**  
  ```bash
  sudo tshark -i eth0 -n -w capture_no_resolve.pcap
  ```  

- **Export Output as JSON**  
  ```bash
  tshark -r capture.pcap -T json > capture.json
  ```  


---

## 8. Additional Resources


- [Tshark Manual Page](https://www.wireshark.org/docs/man-pages/tshark.html) 
