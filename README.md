# Linux Reverse Proxy Infrastructure Lab

Production-style Linux infrastructure lab focused on system administration, networking, reverse proxy configuration, service management, security hardening, logging, and troubleshooting.

---

## Overview

This project simulates a real-world multi-node Linux environment using Ubuntu Server virtual machines. It started as a foundational Linux and networking lab and evolved into a complete infrastructure practice environment designed to reflect Support, DevOps, and SRE scenarios.

The lab demonstrates how infrastructure components interact end-to-end — from a client making a request, through a reverse proxy, to a backend application — with security, logging, and observability built in.

---

## Architecture

```
+----------------+
| Client VM      |
| Test requester |
+-------+--------+
        |
        | HTTP / HTTPS request
        v
+----------------+
| Proxy VM       |
| Nginx          |
| Reverse Proxy  |
| TLS / Logging  |
+-------+--------+
        |
        | Internal HTTP traffic
        v
+----------------+
| App VM         |
| Python App     |
| /health        |
+----------------+
```

| VM | Role | Key Services |
|---|---|---|
| client-vm | External requester | curl, ping, DNS resolution |
| proxy-vm | Public entry point | Nginx, UFW, TLS, rate limiting |
| app-vm | Backend application | Python app, systemd, health endpoint |

---

## What's Running

### Network Connectivity

Multi-VM networking configured with static IPs, hostname resolution, and controlled access between nodes. All VMs verified with zero packet loss.

![Connectivity](docs/screenshots/Connectivity_Works.png)

---

### HTTP → HTTPS Redirect

Nginx configured to redirect all HTTP traffic to HTTPS with a `301 Moved Permanently` response.

![HTTP Redirect](docs/screenshots/HTTP_Redirect.png)

---

### HTTPS Working

TLS termination configured on the proxy. Client receives the backend application response over HTTPS.

![HTTPS Working](docs/screenshots/HTTPS_working.png)

---

### Backend Application — Direct Access

Python web application running on the App VM, serving responses with hostname, client IP, and timestamp. Accessible internally on port 8080.

![Direct Backend Test](docs/screenshots/Direct_backend_test.png)

---

### Nginx — Active & Running

Nginx managed as a systemd service. Running with 4 worker processes and confirmed active since deployment.

![Nginx Status](docs/screenshots/Nginx_Status.png)

---

### Application Service — systemd

Python app deployed and managed with systemd. Service set to auto-start on boot and confirmed active.

![App Status](docs/screenshots/App_Status.png)

---

### Firewall Rules — UFW

Firewall configured on the proxy VM. Only ports 22 (SSH), 80 (HTTP), and 443 (HTTPS) allowed inbound. All other incoming traffic denied by default.

![Firewall Rules](docs/screenshots/Firewall_Rules.png)

---

### Rate Limiting

Nginx rate limiting configured and validated. Requests exceeding the threshold return `503 Service Unavailable`. Verified with a burst test from the client VM.

![Rate Limiting](docs/screenshots/Rate_limiting_test.png)

---

### Nginx Access Logs

Access logs reviewed using `tail -f` on the proxy VM. Logs capture client IP, HTTP method, path, status code, and response size — confirming both successful and rate-limited requests.

![Nginx Logs](docs/screenshots/Nginx_Logs.png)

---

## Technologies Used

| Category | Tools |
|---|---|
| OS | Ubuntu Server |
| Web Server / Proxy | Nginx |
| Backend | Python 3, systemd |
| Security | UFW, SSH key-based auth, TLS |
| Networking | TCP/IP, DNS/host resolution, Netplan, static IPs |
| Scripting | Bash |
| Version Control | Git, GitHub |
| Virtualization | VirtualBox |

---

## Repository Structure

```
.
├── README.md
├── app/
│   └── app.py                    # Python backend application
├── architecture/
│   └── Architecture.md           # Network and component diagram
├── configs/
│   └── nginx-reverse-proxy.conf  # Nginx reverse proxy configuration
├── docs/
│   ├── setup-guide.md            # Step-by-step setup instructions
├── screenshots/                  # Lab evidence and validation captures
└── .gitignore
```

---

## Skills Demonstrated

- Linux system administration (Ubuntu Server)
- Static IP and hostname configuration (Netplan)
- SSH key-based authentication and access hardening
- Nginx reverse proxy configuration
- TLS termination and HTTP → HTTPS redirect
- Rate limiting configuration and validation
- UFW firewall rules (allow/deny by port)
- Python application deployment with systemd
- Service health monitoring (`systemctl status`)
- Log analysis (`tail -f`, access log review)
- Network troubleshooting (`ping`, `curl`, connectivity validation)
- Multi-VM architecture design and documentation
- Technical documentation and operational runbooks

---

## Author

**Adrian Fonseca**
[LinkedIn](https://linkedin.com/in/afc2806) · [GitHub](https://github.com/ODR3N) · [Portfolio](https://odr3n.github.io)
