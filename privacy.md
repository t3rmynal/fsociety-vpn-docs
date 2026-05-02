# Privacy Policy

**Last updated**: 2026-05-02

Plain English. No tracking, no logs, no analytics. We try to keep this honest.

## What we do not keep

On the VPN servers themselves: nothing about you. No source IP. No destination. No DNS query. No URL. No file. No timestamp of when you connected. The servers run with `journald` storage off, no `rsyslog`, no shell history, and every protocol daemon is configured with `log: level: none`.

We do not collect: your real name, your email, your phone, your address, your browsing history, cookies, advertising IDs, device fingerprints, crash reports, or telemetry.

## What we keep

A small set of database rows in our authentication backend (Supabase Postgres) and a few files on your own device.

### In Supabase

Every table is locked down with Row-Level Security: the public anon key cannot read or write any of these directly. Access is only through audited Postgres functions. Operator access uses a separate service-role key kept off the client.

**`keys`** — one row per account.

| column | type | what it holds |
|--------|------|---------------|
| `key_hash` | text (primary key) | SHA-256 of your 16-digit key. The plaintext key never leaves your device. |
| `created_at` | timestamptz | When the account was registered. |
| `sub_until` | timestamptz, nullable | Subscription expiry. Null when no active subscription. |
| `notes` | text, nullable | Operator notes (e.g. "friend referral", written manually). Never auto-populated. |

**`devices`** — one row per device tied to an account, max three per account.

| column | type | what it holds |
|--------|------|---------------|
| `key_hash` | text | Foreign key to `keys.key_hash`. |
| `device_id` | text | Random 16-byte hex generated locally on first connect. |
| `name_words` | text | Three-word human readable name like `Hacker Anonymous Killer`, generated locally. |
| `last_seen` | timestamptz | Updated when the device sends a heartbeat (every few minutes while open). |

**`servers`** — public server inventory, no user data.

| column | type | what it holds |
|--------|------|---------------|
| `id` | text (primary key) | e.g. `europe-central2-warsaw`. |
| `region`, `city`, `country_code` | text | Geographic metadata. |
| `endpoint_host` | text | Public IP of the VPN server. |
| `endpoint_port_awg`, `endpoint_port_hysteria`, `endpoint_port_reality` | int | UDP ports per protocol. |
| `pubkey_awg`, `pubkey_reality` | text | Public keys, safe to share. |
| `load_pct`, `bandwidth_mbps` | int / numeric | Latest load metrics from `fvpn-agent`. Updated every 30 seconds. |
| `online`, `updated_at` | bool / timestamptz | Heartbeat status. |

**`payments`** — one row per Telegram payment confirmation.

| column | type | what it holds |
|--------|------|---------------|
| `id` | uuid (primary key) | Auto-generated. |
| `key_hash` | text | Foreign key to `keys.key_hash`. |
| `tier` | text | `'1m'` or `'3m'`. |
| `amount_usd` | numeric | What you paid. |
| `status` | text | `'pending'`, `'confirmed'`, or `'cancelled'`. |
| `tg_note` | text, nullable | Short operator note linking the payment to a Telegram conversation. Plain text, written by hand. |

**`request_counts`** — sliding-window rate limit counters by IP or key hash. Two-hour rolling retention, automatic cleanup. No personal information beyond the source IP of an API call.

### On your device

- Your 16-digit key, encrypted with the Windows DPAPI on Windows or a 0600 file on Unix-like systems
- Your preferences (chosen protocol, DNS categories, server selection) in plain JSON
- Cached server list (refreshed every 30 seconds)
- Cached privacy and terms documents (refreshed every hour)

Locations: `%LOCALAPPDATA%\fvpn\` on Windows, `$HOME/.config/fvpn/` elsewhere. You can delete them at any time.

## Honest limits

The Supabase backend sees the public IP address of the device making the API call. We are direct-to-Supabase in v0.1, with no Cloudflare proxy in front. This is documented openly rather than promised against. Optional onion routing of API calls through your own active VPN session is on the v1.5 roadmap.

The hosting provider (Google Cloud) sees the operator's billing identity. They do not see VPN traffic content but they do know the operator runs VPN servers. Migration to anonymous-friendly hosting (Njalla, 1984 Hosting) with Monero payment is on the roadmap.

## Servers

Four European servers, intentionally narrow geographic spread:

| city | country | region | jurisdiction |
|------|---------|--------|--------------|
| Warsaw | Poland | `europe-central2` | EU GDPR |
| Stockholm | Sweden | `europe-north2` | EU GDPR + Swedish privacy laws |
| Zurich | Switzerland | `europe-west6` | Swiss FADP, outside EU |
| Amsterdam | Netherlands | `europe-west4` | EU GDPR + strong Dutch privacy tradition |

All four are managed Google Cloud VMs. Routing your traffic through them does not change the legal exposure of what you do, but it does change the network location your destinations see.

## Retention

| data | retention | how it gets deleted |
|------|-----------|---------------------|
| Account key hash, devices, payments | While account exists | Logout from all devices, or 30-day inactivity after subscription expiry, or message [@aimwork](https://t.me/aimwork) |
| Server load metrics | 90 days rolling | Automatic |
| Rate limit counters | 2 hours rolling | Automatic |
| Telegram support messages | 30 days after resolution | Manual |

## Third parties

| processor | role | sees |
|-----------|------|------|
| Google Cloud | Hosts the four VPN servers | Server uptime, operator billing identity. No VPN traffic content. |
| Supabase | Hosts the auth + subscription database | Key hashes, device IDs, server metrics, IP addresses of API calls. |
| GitHub | Hosts source and binary releases | Download counts, commit metadata. |
| Telegram | Carries operator support conversations | Whatever you write in chat. |

## Your rights (GDPR)

You can ask for: a copy of all data tied to your hash, deletion of your account, correction of any wrong data, a JSON export, restriction of processing during a dispute. First request in any 12-month window is free, replies within 7 days. Contact [@aimwork](https://t.me/aimwork) on Telegram with your key hash (not your plaintext key, which we do not store).

If you think your rights have been violated you can complain to your local data protection authority. In Germany that is the [BfDI](https://www.bfdi.bund.de/).

## Children

The Service is not directed at children under 13. The operator is currently a minor and operates the Service with the consent of their legal guardian; this may affect legal recourse against the Service in some jurisdictions.

## Changes

The "Last updated" date above changes when this document changes. Material changes (what we collect, who has access, retention) will be announced in the project Telegram channel and on the home screen of the application. The application fetches the latest version on every launch.

## Contact

- Telegram: [@aimwork](https://t.me/aimwork)
- GitHub: [t3rmynal/fsociety-vpn/issues](https://github.com/t3rmynal/fsociety-vpn/issues)
- Source of this document: [fsociety-vpn-docs/privacy.md](https://github.com/t3rmynal/fsociety-vpn-docs/blob/main/privacy.md)
