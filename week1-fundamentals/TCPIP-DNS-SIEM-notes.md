# Week 1 Lab Notes – Fundamentals Review
*Operation Sentinel – Learning in Public*
*Date: May 2026 | Focus: IAM foundations*

## 1. TCP/IP Basics (Refresh)
**What I reviewed:**
- OSI Layer 3 (Network) vs Layer 4 (Transport)
- TCP three-way handshake: SYN → SYN-ACK → ACK
- Why this matters for IAM: every login, every API call rides TCP. If I don't understand the handshake, I can't spot abnormal session patterns.

**Command I ran:**
```bash
# Basic connectivity check
ping 8.8.8.8
traceroute google.com
Takeaway: Latency spikes during auth can indicate proxy issues or MITM. Document baseline.

2. DNS Basics
What I reviewed:

DNS resolution flow: client → recursive resolver → root → TLD → authoritative
A record vs CNAME vs MX
DNS tunneling as exfiltration method
Command I ran:
# Check DNS resolution
nslookup github.com
dig +short github.com
AM link: Orphaned service accounts often use old DNS names. If DNS fails, auth fails. Check DNS before blaming IAM.

3. SIEM Log Parsing – Intro
What I reviewed:

Common log sources for IAM: Windows Event ID 4624 (logon), 4625 (failed), 4769 (Kerberos)
Basic Splunk search syntax: index=main sourcetype=WinEventLog | stats count by user
What I practiced:

Parsed a sample auth.log file
Looked for: failed logins from same IP, off-hours access, service account usage
Sample query I wrote:
index=iam_logs EventCode=4625 | table _time, user, src_ip, failure_reason | sort -_time
Takeaway: SIEM is just a magnifying glass for IAM behavior. Start with one event code, master it.

