# &nbsp;Cybersecurity Incident Report: Network Traffic Analysis



### Typology of the TCPDump Log Data

The tcpdump output contains several structural elements that describe how the DNS request and ICMP error responses were transmitted. These elements help classify the traffic and understand the failure.



### Timestamp

* Each entry begins with a timestamp such as **13:24:32.192571**.
* Indicates the precise moment the packet was observed.
* Format: **HH:MM:SS.microseconds**



### Source and Destination IP Addresses

* Outgoing DNS request:

  * **Source:** 192.51.100.15 (client machine)
  * **Destination:** 203.0.113.2 (DNS server)

* ICMP error response:

  * **Source:** 203.0.113.2 (DNS server or intermediate device)
  * **Destination:** 192.51.100.15 (client)



### Protocols Observed

* **UDP**

  * Used for sending DNS queries to port 53.

* **ICMP**

  * Used to return error messages when delivery fails.



### Ports and Services

* **UDP port 53**

  * Standard port for DNS queries.
  * Marked as unreachable in the ICMP response.



### DNS Query Metadata

* **Query Identification Number:** `35084`

  * A unique identifier assigned by the client to match DNS responses to DNS requests.

* **DNS Query Flags:** `A?`

  * Indicates a request for an **A record**, which maps a domain name to an IPv4 address.

* **Query Purpose:**

  * The browser attempted to resolve the domain name `yummyrecipesforme.com` to its IP address.

* \**Packet Structure Notes:*

  * The `+` symbol after the query ID indicates additional DNS flags were set.
  * The `(24)` at the end of the line represents the size of the DNS query payload in bytes.



### Error Type

* ICMP message: **“udp port 53 unreachable”**
* Indicates the DNS request could not reach a listening DNS service on the destination system.

---



## Part 1: Summary of the Problem Found in the DNS and ICMP Traffic Log



The network analysis shows that DNS requests sent via UDP to the DNS server at **203.0.113.2** were unsuccessful. Each DNS query resulted in an ICMP error message indicating:

* **“udp port 53 unreachable”**

  * Port **53** is the standard port used for DNS services. The “unreachable” response indicates that the DNS request did not reach a functioning DNS service on the destination system.\\



### Possible Issues Identified

* Incorrect DNS server IP address
* DNS service not running or crashed
* Firewall blocking UDP port 53 (inbound or outbound)
* DNS server overloaded or under a DDoS attack
* Routing issues preventing packets from reaching the DNS server
* DNS server configured to listen only on TCP instead of UDP
* NAT device not forwarding port 53 correctly
* IDS/IPS security appliance blocking DNS traffic
* DNS server using a non‑standard port configuration

---



## Part 2: Analysis of the Data and Likely Cause of the Incident



### Time of Incident

* **1:24:32.19 PM**, based on the timestamp in the tcpdump logs.



### How the IT Team Became Aware

* Customers reported that they were unable to reach the client website.



### Actions Taken by the IT Department

* Loaded tcpdump to analyze live network traffic.
* Attempted to load the website again, triggering:

  * A DNS request sent via UDP to obtain the website’s IP address.
  * ICMP error responses indicating port 53 was unreachable.



### Key Findings from the Investigation

* Port **53** on the DNS server was unreachable.
* ICMP “port unreachable” messages were returned repeatedly.
* No DNS service appeared to be listening on the receiving end.
* Multiple attempts produced identical failures.
* No valid DNS response was received for the requested domain.
* DNS query metadata (ID 35084, A? flag) confirms the request was a standard A‑record lookup.



### Likely Causes of the Incident

* Incorrect DNS server IP address
* DNS service outage or misconfiguration
* Firewall blocking DNS traffic
* DNS server overwhelmed or under DDoS attack
* Routing failure between client and DNS server
* DNS server listening only on TCP
* NAT misconfiguration preventing proper forwarding
* IDS/IPS blocking DNS queries
* DNS server using a non‑standard port
