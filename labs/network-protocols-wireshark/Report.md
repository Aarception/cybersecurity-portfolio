# Lab Report: Wireshark and Linux Network Diagnostics

## Objective
Demonstrate proficiency in network protocol analysis using Wireshark and Linux command-line tools, including `ping`, `traceroute`, `arp`, and `nslookup`, while applying filters and interpreting traffic.

---

## Task 1: Launch Wireshark
**Command:**  
`sudo wireshark`

**Observation:**  
- Wireshark launches with root privileges.  

![Wireshark Launch](screenshots/Image (1).png)

---

## Task 2: Select Interface
**Action:**  
- Double-click on `eth0` when Wireshark pops up.  

**Observation:**  
- Capture begins on the selected interface.  

![Interface Selection](screenshots/Image (2).png)

---

## Task 3: Generate Browser Traffic
**Action:**  
- Open Firefox and visit:  
  - www.google.com  
  - www.wikipedia.org  
  - www.skillspire.net  

**Observation:**  
- Pages load successfully, generating HTTP/DNS traffic.  

![Browser Traffic](screenshots/Image (3).png)

---

## Task 4: Stop Capture and View Packets
**Action:**  
- Stop capture after browsing.  

**Observation:**  
- A list of captured packets is displayed in Wireshark.  

![Captured Packets](screenshots/Image (4).png)

---

## Task 5: Apply HTTP Filter
**Filter:**  
`http`

**Observation:**  
- Only HTTP traffic is displayed.  

![HTTP Filter](screenshots/Image (5).png)

---

## Task 6: Apply DNS Filter
**Filter:**  
`dns`

**Observation:**  
- Only DNS traffic is displayed.  

![DNS Filter](screenshots/Image (6).png)

---

## Task 7–9: Ping Tests
**Commands:**  
`ping www.google.com`  
`ping www.wikipedia.com`  
`ping www.tesla.com`

**Observation:**  
- All sites responded with 0% packet loss.  
- RTT values varied across domains.  

![Ping Google](screenshots/Image (7).png)  
![Ping Wikipedia](screenshots/Image (8).png)  
![Ping Tesla](screenshots/Image (9).png)

---

## Task 10: Apply ICMP Filter
**Filter:**  
`icmp`

**Observation:**  
- ICMP echo requests and replies are displayed.  

![ICMP Filter](screenshots/Image (10).png)

---

## Task 11: Analyze DNS Queries and Responses
**Observation:**  
- DNS queries for A and AAAA records.  
- Responses include IPv4 and IPv6 addresses.  

![DNS Query Response](screenshots/Image (11).png)

---

## Task 12: Follow UDP Stream
**Action:**  
- Right-click DNS packet → Follow → UDP Stream.  

**Observation:**  
- ASCII stream shows Tesla domain resolution via Akamai CDN.  

![UDP Stream](screenshots/Image (12).png)

---

## Task 13: Filter by IP Address
**Filter:**  
`ip.addr == <YOUR_IP_ADDRESS>`  

**Observation:**  
- Displays traffic to/from the specified IP.  

![IP Filter](screenshots/Image (13).png)

---

## Task 14: ARP Table
**Command:**  
`arp -a`

**Observation:**  
- Shows MAC/IP mapping for local devices.  

![ARP Table](screenshots/Image (14).png)

---

## Task 15: DNS Lookup
**Command:**  
`nslookup google.com`

**Observation:**  
- Resolves to IPv4 and IPv6 addresses.  

![NSLookup](screenshots/Image (15).png)

---

## Task 16: Traceroute
**Command:**  
`traceroute google.com`

**Observation:**  
- First hop responds, subsequent hops time out.  

![Traceroute](screenshots/Image (16).png)

---

## Task 17: Filtered Traffic Analysis
**Action:**  
- Apply IP filter and take screenshot.  

**Observation:**  
- Traffic shows ICMP requests/replies.  
- Interpretation: No failed packets, normal latency.  

![Filtered Traffic](screenshots/Image (17).png)

---

## Challenges & Resolution

| **Challenge**              | **Cause**                                  | **Resolution**                                 |
|-----------------------------|---------------------------------------------|------------------------------------------------|
| No packets in Wireshark     | Capture started before traffic generation   | Revisited sites after capture start            |
| Traceroute incomplete       | Intermediate routers did not respond        | Noted limitation and used ping for RTT         |
| ARP interpretation          | MAC/IP mapping unclear                      | Compared with Wireshark ARP frame              |

---

## Conclusion
This lab demonstrated:
- Real-time packet capture and filtering in Wireshark.  
- Use of Linux tools (`ping`, `traceroute`, `arp`, `nslookup`) for diagnostics.  
- Analysis of ICMP, DNS, and UDP traffic.  
- Interpretation of packet-level data and stream content.  

---