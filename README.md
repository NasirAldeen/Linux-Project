# 🐧 Linux Project — DNS & Proxy Server Configuration

> **Linux Course Lab Assignment**
> Arab American University (AAUP) — Nablus, West Bank, Palestine

This repository contains the full lab report documenting the setup and configuration of two network services on a **Fedora Linux** virtual machine for a fictional local company domain `XYZ.local`.

**Student:** Mohammad Baker | **ID:** 201912050
**Course:** Linux | **Section:** 2 | **Department:** Computer Systems Engineering
**Instructor:** Dr. Allam Abu Maise
**Submission Date:** 12/06/2026

---

## 📋 Project Overview

The lab covers the complete configuration of two server services on a Fedora VM:

- **BIND DNS Server** — Authoritative DNS for the `XYZ.local` domain
- **Squid Proxy Server** — ACL-based HTTP proxy with caching and access control

---

## 🌐 Part 1: BIND DNS Server

### Domain & Network

| Parameter | Value |
|-----------|-------|
| Domain | `XYZ.local` |
| Subnet | `192.168.64.0/24` |
| Server Role | Authoritative DNS |

### Installation

```bash
dnf install bind bind-utils
```

### Forward Zone — A Records

| Hostname | IP Address |
|----------|------------|
| dns | 192.168.64.10 |
| web | 192.168.64.20 |
| mail | 192.168.64.30 |
| proxy | 192.168.64.40 |
| db | 192.168.64.50 |

### Configuration Files

| File | Purpose |
|------|---------|
| `/etc/named.conf` | Main BIND configuration |
| `/var/named/<forward-zone-file>` | Forward zone (A records) |
| `/var/named/<reverse-zone-file>` | Reverse zone (PTR records) |

### Validation & Testing

- `named-checkconf` — validates `named.conf` syntax
- `named-checkzone` — validates zone file syntax
- `firewall-cmd` — opens DNS port in the firewall
- `nslookup` — forward and reverse lookup testing

---

## 🔒 Part 2: Squid Proxy Server

### Installation

```bash
dnf install squid
```

### Configuration — Port & ACL Rules

**Listening Port:** `3128`

| Rule | Action |
|------|--------|
| Allow all of `192.168.1.0/24` | ✅ Full access |
| Allow only `192.168.7.10` and `192.168.7.20` from `192.168.7.0/24` | ✅ Specific hosts only |
| Block `facebook.com` for everyone | ❌ Denied (all) |
| Block `instagram.com` for `192.168.1.0/24` | ❌ Denied (subnet) |

ACL rules use `dstdom_regex` for domain blocking inside `squid.conf`.

### Testing

**curl-based testing:**
- Allowed sites → `HTTP 200 OK`
- Blocked sites → `HTTP 403 Forbidden`

**Browser testing:**
- Firefox proxy configured to use Squid on port 3128

### Caching Demonstration

A local Python HTTP server served a 10 MB test file to demonstrate Squid's caching behaviour:

| Request | Cache Log |
|---------|-----------|
| First request | `TCP_MISS` (fetched from origin) |
| Second request | `TCP_HIT` (served from cache) |

Cache storage configured via `cache_dir ufs` in `squid.conf`.

---

## 🛠️ Tools & Technologies

| Tool | Purpose |
|------|---------|
| **Fedora Linux (VM)** | Host operating system |
| **BIND** | DNS server (`dnf install bind bind-utils`) |
| **Squid** | Proxy server (`dnf install squid`) |
| **firewall-cmd** | Firewall rule management |
| **nslookup** | DNS lookup testing |
| **curl** | HTTP proxy testing |
| **Python HTTP server** | Caching demonstration |
| **Firefox** | Browser proxy configuration testing |

---

## 👤 Author

**Mohammad Baker**
Computer Systems Engineering Student — Arab American University (AAUP)
Nablus, West Bank, Palestine
[LinkedIn](https://www.linkedin.com/in/nasiraldeen-ishtaiah/)
