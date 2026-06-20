# Pi-hole DNS Sinkhole — Home Lab

![Phase 1](https://img.shields.io/badge/Phase%201-Complete-success)
![Phase 2](https://img.shields.io/badge/Phase%202-In%20Progress-yellow)
![Platform](https://img.shields.io/badge/Platform-Ubuntu%2022.04-orange)
![Docker](https://img.shields.io/badge/Docker-Compose-blue)

## Objective

Deploy a network-level DNS sinkhole on Ubuntu Server using Docker, validate it
on individual devices, then roll it out network-wide via router DHCP. This
lab was deployed on a server already running a Wazuh SIEM stack and a
ModSecurity/Nginx WAF — a real multi-service integration challenge, not a
blank-slate install.

![Pi-hole dashboard](screenshots/09-dashboard-live.png)

## Status

**Phase 1 — Complete.** Pi-hole deployed in Docker, six real deployment
problems diagnosed and resolved, DNSSEC validated, manually tested on 2
personal devices.

**Phase 2 — In progress.** Rolling Pi-hole out as the default DNS resolver
for the entire home network via router DHCP, covering every device
automatically without per-device configuration.

## Results So Far

- 85,222 domains on blocklist (StevenBlack + URLhaus malware feed + smart TV telemetry list)
- DNSSEC validated — confirmed via the `ad` (Authenticated Data) flag in `dig` output
- 6 deployment problems identified from real error output and resolved (see below)
- Smart TV / mobile ad and telemetry domains blocked cleanly
- YouTube and Netflix ad blocking tested and confirmed **not reliably possible** via DNS — documented honestly with the technical reason (same-domain ad insertion)

## Repository Structure

```
pihole-dns-lab/
├── README.md                          this file
├── docker-compose.yml                 sanitized compose file (uses .env, no hardcoded secrets)
├── .env.example                       template — copy to .env and fill in your own values
├── .gitignore                         excludes .env, runtime volumes, logs
├── setup.sh                           automated setup script, encodes all Phase 1 fixes
├── screenshots/                       11 evidence screenshots (IP/password/hostname masked)
├── volumes/pihole/etc-dnsmasq.d/
│   └── 99-dnssec.conf                 the actual config file that fixed DNSSEC validation
└── docs/
    ├── PHASE1-troubleshooting.md      full breakdown of all 6 problems + fixes
    └── PHASE2-network-rollout.md      router DHCP rollout, failover, IoT handling
```

## Quick Start

```bash
git clone <this-repo>
cd pihole-dns-lab
chmod +x setup.sh
./setup.sh
```

The script handles `.env` creation, YAML validation, the port 53 conflict,
Docker installation, firewall rules, and deployment — all fixes from Phase 1
are built in so you don't have to rediscover them.

## Environment

| Component | Detail |
|---|---|
| Host OS | Ubuntu Server 22.04 LTS |
| Containerisation | Docker + Docker Compose |
| Pi-hole version | pihole/pihole:latest (v6.x) |
| Upstream DNS | Cloudflare 1.1.1.1 + Google 8.8.8.8 |
| DNSSEC | Enabled and validated |
| Pre-existing services | Wazuh SIEM, ModSecurity/Nginx WAF |
| Web UI port | 8081 (80 was already in use) |

## The Six Problems (Phase 1)

| # | Problem | Root Cause | Fix |
|---|---------|-----------|-----|
| 1 | YAML parse error at line 38 | Indentation error in `ports:` block | Fixed indent; validated with `docker compose config` |
| 2 | Port 80 already allocated | Existing Nginx WAF owned host port 80 | Remapped Pi-hole web UI to port 8081 |
| 3 | Container unhealthy, DNS unavailable | UID mismatch on mounted volumes | Added `PIHOLE_UID`/`PIHOLE_GID` + top-level `dns:` block |
| 4 | `pihole -a adlist add` silent failure | Command removed in Pi-hole v6 | Used `pihole-FTL sqlite3` to insert directly into `gravity.db` |
| 5 | Web UI unreachable on port 8081 | UFW not allowing the new port | `sudo ufw allow 8081/tcp` |
| 6 | DNSSEC set to `true` but `ad` flag missing | Env var alone insufficient in v6 | Added explicit `99-dnssec.conf` to `dnsmasq.d` |

Full diagnosis, screenshots, and exact commands for each: [docs/PHASE1-troubleshooting.md](docs/PHASE1-troubleshooting.md)

## What's Next (Phase 2)

Moving from 2 manually-configured devices to full network coverage via
router DHCP, including failover planning for what happens if Pi-hole goes
down, and handling devices (IoT, smart TVs) that hardcode their own DNS.

Full plan: [docs/PHASE2-network-rollout.md](docs/PHASE2-network-rollout.md)

## Known Limitations (Documented Honestly)

DNS-level filtering cannot block ads on platforms that serve ads and content
from the same domain — most notably YouTube and Netflix. This is not a
missing blocklist; it's a structural limit of the OSI layer at which DNS
operates. Smart TV platform telemetry (Roku, Samsung, LG ad/tracking
domains) blocks reliably because those domains are kept separate from
content delivery.

## Security+ SY0-701 Coverage

| Activity | Domain | Objective |
|---|---|---|
| DNS sinkhole as defensive control | D4 — Network Security | 4.4 — Network hardening |
| DNSSEC validation | D4 — Network Security | 4.5 — DNS attacks |
| Router DHCP rollout | D4 — Network Security | 4.1 — Network architecture |
| Single point of failure mitigation | D5 — Program Management | 5.2 — Risk management |
| URLhaus threat intel feed | D2 — Vulnerabilities | 2.1 — Threat intelligence |
| Docker container isolation | D3 — Architecture | 3.1 — Secure infrastructure |

## License

MIT — use freely for your own home lab.
