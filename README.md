# Lab 01 — Azure Static Website Hosting

**Series:** Azure Cloud Security Labs
**Author:** Javen

---

## Overview

Deployed a public-facing static website using Azure Blob Storage instead of a Virtual Machine.

A VM runs an OS and web server process 24/7 — even when no one visits the site. For a static website with no server-side logic, that's unnecessary overhead. With Azure Blob Storage Static Website hosting, there's no OS to manage, no web server running in the background, and no dedicated compute. Azure serves the files directly on request.

This is PaaS (Platform as a Service) in practice — you own the content, Azure owns the infrastructure.

---

## Architecture

```
User → HTTPS → Public Endpoint → Storage Account → $web Container → index.html
```

When Static Website Hosting is enabled, Azure automatically creates a `$web` container. This is the **only** location Azure's public endpoint will serve files from. Uploading files to any other container results in a 404 — Azure won't look there.

---

## Resources Deployed

| Resource | Name | Config |
|---|---|---|
| Resource Group | rg-lab01-[name] | East US |
| Storage Account | stlab01[name] | Standard, LRS |
| Container | $web | Auto-created |

---

## Key Decision — Storage Redundancy

LRS (Locally Redundant Storage) was selected for this lab — the lowest cost option.

| Tier | Copies | Scope |
|---|---|---|
| LRS | 3 | Same datacenter |
| ZRS | 3 | 1 per availability zone, same region |
| GRS | 6 | 3 in primary region + 3 in secondary region |

LRS is fine for a lab. In production, it's a single point of failure — one datacenter outage takes all 3 copies down simultaneously. The redundancy choice is a business continuity decision, not just a cost one.

---

## Naming Conventions

Resources follow a structured naming standard — `rg-lab01-[name]`, `stlab01[name]`.

In an enterprise environment with hundreds of resources across multiple environments, naming conventions tell you instantly what a resource is, where it belongs, and who owns it. During incident response, that clarity matters.

---

## Security Consideration — Public Access

Public blob access is required for this lab to work. In production, a misconfigured storage account with public access is a serious risk — exposing PII, PHI, trade secrets, or internal credentials.

At scale this is managed through:
- **Microsoft Defender for Cloud** — continuously scans for misconfigurations against industry benchmarks (MCSB, CIS, NIST), no manual setup required
- **Azure Policy** — enforces access rules across subscriptions, blocking non-compliant resources before they're created

Manual review doesn't scale. Policy and automation do.

---

## Clean Up

All resources deleted via Resource Group removal post-lab.

In enterprise environments, orphaned resources are a real problem:
- **Cost** — unused infrastructure accumulates silently at scale
- **Security** — forgotten resources fall outside active monitoring, expanding your attack surface
- **Compliance** — unaccounted resources touching sensitive data are a liability in regulated industries

Resource lifecycle management is as important as deployment.

---

## What I'd Do Differently

I moved past the LRS selection without thinking it through. In a real deployment I'd pause on every configuration decision and ask — *what does this mean in production at scale?*

That's the habit this lab series is building.

---

## Files

| File | Description |
|---|---|
| `index.html` | Static website file deployed in the lab |
| `architecture.html` | Visual architecture diagram |

---

*Lab 01 of ongoing Azure Cloud Security hands-on series.*
*Follow the journey on [LinkedIn](https://www.linkedin.com/in/javen-m-732a1115a/)*
