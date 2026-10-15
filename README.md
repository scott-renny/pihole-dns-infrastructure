<div align="center">

# Pi-hole DNS Infrastructure
### DNS Filtering · Docker · Ubuntu Server

![status: legacy project](https://img.shields.io/badge/status-legacy%20project-64748b?style=flat-square)
![platform: Ubuntu](https://img.shields.io/badge/platform-Ubuntu-2563eb?style=flat-square)

[Architecture](ARCHITECTURE.md) · [License](LICENSE)

</div>

[Enterprise DNS Filtering with Pi-hole, Docker & Ubuntu Server](#enterprise-dns-filtering-with-pi-hole-docker--ubuntu-server) · [Project Overview](#project-overview) · [Project at a Glance](#project-at-a-glance) · [Architecture](#architecture) · [Engineering Decisions](#engineering-decisions) · [Key Accomplishments](#key-accomplishments)

---

## Enterprise DNS Filtering with Pi-hole, Docker & Ubuntu Server

> [!IMPORTANT]
> ### 📦 Legacy Engineering Portfolio Project
>
> This repository documents one of the infrastructure engineering projects I completed while developing my home lab.
>
> Rather than deleting or replacing it, I have intentionally preserved it as part of my **Legacy Project Archive** to document my growth as an infrastructure and cybersecurity engineer.
>
> This project demonstrates the deployment of a Docker-based Pi-hole DNS sinkhole integrated into an existing Ubuntu Server already hosting Wazuh SIEM, ModSecurity, and Nginx.
>
> Every troubleshooting step, deployment issue, engineering decision, and lesson learned has been preserved to document the engineering process—not just the finished solution.
>
> My current engineering efforts are focused on:
>
> - 🛡️ Cyber Operations Center Engineering Program *(Flagship Project)*
> - 🏗️ Project Atlas
> - 📡 [NET-WATCH](https://github.com/scott-renny/netwatch)
> - 🪖 [Project Hermes](https://github.com/scott-renny/project-hermes)
> - 🐺 [Project Cerberus](https://github.com/scott-renny/project-cerberus-build)
> - ⚔️ [Project Ares](https://github.com/scott-renny/project_ares)
> - ☀️ [Project Apollo](https://github.com/scott-renny/project-apollo)

---


# Project Overview

This project documents the deployment of a Docker-based Pi-hole DNS sinkhole inside an existing multi-service Ubuntu Server environment. Rather than deploying Pi-hole on a blank server, the objective was to integrate DNS filtering alongside Wazuh SIEM, ModSecurity, Nginx, Docker, and existing firewall rules while maintaining service stability.

## Project at a Glance

| Category | Details |
|---|---|
| Project Type | Infrastructure Engineering |
| Status | Legacy Portfolio Project |
| Platform | Ubuntu Server 22.04 LTS |
| Deployment | Docker |
| Focus | DNS Security |
| Documentation | Complete |
| Troubleshooting | Documented |
| Engineering Phases | 2 |

# Architecture

A detailed view of the container, network and DNS query path is in [ARCHITECTURE.md](ARCHITECTURE.md).

```text
                    Internet
                        │
                        ▼
                Router / Firewall
                        │
                        ▼
                Ubuntu Server 22.04
                        │
        ┌───────────────┼────────────────┐
        │               │                │
        ▼               ▼                ▼
    Pi-hole         Wazuh SIEM      ModSecurity
 DNS Filtering      Monitoring      + Nginx WAF
        │
        ▼
 Home Network Devices
```

# Engineering Decisions

- **Docker** for service isolation, portability, and easier maintenance.
- **Pi-hole** for mature DNS filtering and DNSSEC support.
- **Cloudflare + Google DNS** for resilient upstream resolution.
- **Integration with an existing server** to simulate real infrastructure instead of a clean installation.

# Key Accomplishments

- Successfully deployed Pi-hole using Docker.
- Integrated DNS filtering into an existing security stack.
- Validated DNSSEC.
- Diagnosed and resolved six deployment issues.
- Built reusable deployment automation.
- Produced repeatable documentation.

# Technologies Used

- Ubuntu Server
- Docker & Docker Compose
- Pi-hole
- DNSSEC
- UFW
- Wazuh
- ModSecurity
- Nginx
- Cloudflare DNS
- Google Public DNS

# Skills Demonstrated

- Linux Administration
- Infrastructure Engineering
- DNS Administration
- Docker
- Network Security
- Secure Configuration
- Troubleshooting
- Technical Documentation
- Deployment Automation

# Repository Structure

```text
pihole-dns-lab/
├── README.md
├── docker-compose.yml
├── setup.sh
├── .env.example
├── screenshots/
├── docs/
└── volumes/
```

# Project Results

- 85,000+ blocked domains
- DNSSEC validated
- Six documented deployment incidents
- Smart TV telemetry blocked
- Malware blocklists integrated
- Successfully integrated with an existing security infrastructure

# Engineering Challenges

- Existing services occupied required ports.
- DNSSEC required manual configuration.
- Pi-hole v6 removed documented commands.
- Docker networking required troubleshooting.
- Firewall rules required modification.

# Documentation

Includes deployment guides, troubleshooting documentation, screenshots, configuration files, rollout planning, and lessons learned.

# Retrospective

If I rebuilt this project today I would:

- Deploy redundant Pi-hole instances
- Add automatic backups
- Implement DNS over HTTPS/TLS
- Add health monitoring
- Integrate centralized dashboards
- Automate configuration management

# Future Improvements

Potential enhancements:

- High Availability DNS
- Grafana dashboards
- Discord alerting
- Automated health checks
- Configuration management

# Security+ Coverage

- Network Security
- DNS Security
- Secure Infrastructure
- Threat Intelligence
- Risk Management

# Engineering Philosophy

Every project in my Legacy Project Archive has been preserved intentionally.

Rather than showcasing only polished final results, I believe documenting failures, troubleshooting, and engineering decisions provides a more accurate representation of how real-world infrastructure is built and maintained.

These repositories represent the foundation upon which my current engineering portfolio has been built.

# Current Engineering Focus

- 🛡️ Cyber Operations Center Engineering Program *(Flagship Project)*
- 🏗️ Project Atlas
- 📡 [NET-WATCH](https://github.com/scott-renny/netwatch)
- 🪖 [Project Hermes](https://github.com/scott-renny/project-hermes)
- 🐺 [Project Cerberus](https://github.com/scott-renny/project-cerberus-build)
- ⚔️ [Project Ares](https://github.com/scott-renny/project_ares)
- ☀️ [Project Apollo](https://github.com/scott-renny/project-apollo)

# License

MIT License

# Author

## Scott Renny

**Security+ Certified • Cybersecurity and Infrastructure Engineering**

*"Building enterprise infrastructure one project at a time while continuously learning, improving, and documenting the journey."*
