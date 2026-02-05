**## 

| Goal | Command |
| --- | --- |
| DNS only | `nslookup service` |
| Network reachability | `ping` |
| Port open | `nc -zv host port` |
| HTTP works | `curl` |
| Debug HTTP | `curl -v` |
| Headers only | `curl -I` |
| Download data | `wget` |
| Local app | `curl 127.0.0.1:port` |

---

## Final Pro Tip (real-world workflow)

When something breaks, **don’t panic** — do this:

1. `nslookup service`
2. `nc -zv service port`
3. `curl service`
4. `curl -v service`

### 🔧 Connectivity Testing – Quick Notes (DevOps View)

- **`ping <host>`** → Tests **basic network reachability (ICMP)**. Does *not* confirm ports or services.
- **`nslookup <name>`** → Checks **DNS resolution** (CoreDNS in Kubernetes). Name → IP only.
- **`curl <url>`** → Tests **HTTP/HTTPS connectivity** (DNS + network + port + app).
- **`curl -v <url>`** → Same as `curl` but shows **detailed request/response & TLS info**.
- **`curl -I <url>`** → Fetches **HTTP headers only** (fast health check).
- **`nc -zv <ip> <port>`** → Verifies if a **TCP port is open** (no application logic).
- **`wget -O- <url>`** → Downloads content to stdout; useful in **minimal containers**.
- **`telnet <ip> <port>`** → Legacy **raw TCP connectivity** test (mostly replaced by `nc`).

### Kubernetes Notes

- **Service names & ExternalName** work **only inside the cluster**.
- To test ExternalName targets, hit the **actual external endpoint** directly.

### Rule of Thumb

- Use **`curl`** for web services
- Use **`nc`** for port checks
- Use **`nslookup`** for DNS
- Use **`ping`** for basic reachability**
