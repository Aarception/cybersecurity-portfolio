# Cybersecurity Incident Report: Hardening Against Brute Force

## Part 1: Summary of the Problem Found in the TCP Traffic Log

The workstation first communicated with `yummyrecipesforme.com` after resolving it to **203.0.113.22**.

![Hardening](screenshots/image%20(1).png)

 A normal TCP handshake and HTTP GET request followed, and traffic continued without anomalies.

![Hardening](screenshots/image%20(2).png)


Minutes later, the workstation issued a second DNS query for `greatrecipesforme.com`, which resolved to **192.0.2.172**. A new TCP session was created from a different ephemeral port, and another HTTP GET request was sent.


![Hardening](screenshots/image%20(4).png)

 The abrupt shift between two similarly named domains, combined with the change in destination IP space, indicates a likely redirection event rather than normal browsing behavior.

The sequence suggests the user was moved—intentionally or through malicious content—from the first domain to a second, suspiciously similar one.

---

## Part 2: Analysis of the Data and Likely Cause of the Incident

### Timeline Overview

* **14:18:32** — DNS lookup for `yummyrecipesforme.com`
* **14:18:36** — TCP handshake + HTTP GET to **203.0.113.22**
* **14:20:32** — DNS lookup for `greatrecipesforme.com`
* **14:25:29** — TCP handshake + HTTP GET to **192.0.2.172**

![Hardening](screenshots/image%20(3).png)

### Key Observations

* Both domains share a naming pattern that suggests impersonation or typo‑squatting.
* The second domain appears shortly after normal traffic, consistent with a redirect or malicious script execution.
* The workstation reused the same DNS source port (`52444`), indicating both lookups originated from the same process.
* The second IP belongs to a different address block, reinforcing the likelihood of a spoofed or malicious destination.
* No handshake failures or resets appear — the traffic is clean, which aligns with controlled redirection rather than network instability.

![Hardening](screenshots/image%20(5).png)

### Likely Cause

The traffic pattern is consistent with a **malicious redirection** triggered by the first site or by user interaction with deceptive content. The second domain’s naming similarity and distinct IP range strongly suggest a spoofed site designed to mimic the first.

---

## Part 3: Recommended Remediation for Brute‑Force Attacks

Apply **rate limiting** to authentication attempts to restrict repeated login attempts from automated tools.



