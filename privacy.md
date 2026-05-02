# Privacy Policy

**Last updated**: 2026-05-02

This Privacy Policy explains what information fsociety vpn (the "Service") collects when you use it, what information it does not collect, how that information is used, who has access to it, and the rights you have over it.

We aim to be unambiguous and honest. Where the Service does something the user might not expect, we say so plainly rather than burying it in legalese. Where a privacy guarantee has limits, we state those limits openly.

## 1. Who we are

fsociety vpn is operated by an individual based in the European Union. The Service is currently in alpha and is operated as a personal project, not as a registered legal entity. Communication and payment confirmation goes through Telegram at [@aimwork](https://t.me/aimwork).

This privacy policy will be updated when the Service is incorporated as a legal entity, or if any of the operational practices described here change. The "Last updated" date above reflects the most recent revision.

## 2. The short version

- We do not log, store, or share your VPN traffic. Not source IP addresses to which you connect, not destination addresses, not DNS queries, not domain names, not packet contents, not connection durations, not bandwidth used per session.
- We do log the SHA-256 hash of your account key, the timestamp at which each of your devices last contacted our authentication backend, and aggregated server load metrics (CPU and total network throughput per server). We retain this data while your subscription is active and for thirty days after expiry.
- Our authentication backend (Supabase) sees the public IP address of the device making the API call. This is documented honestly here rather than promised against. Mitigation through optional onion routing of API calls through your own active VPN session is on the v1.5 roadmap.
- We use a small number of third parties: Google Cloud Platform (VPS hosting), Supabase (authentication and subscription database), GitHub (binary release distribution). None of them have access to your VPN traffic.
- Payments through Telegram are pseudonymous. We do not retain financial information beyond a confirmation note linked to your account key hash.

The remainder of this policy explains each of these points in detail and discloses the limits.

## 3. Information we collect

### 3.1 On your device (client side, never transmitted)

Stored locally only, never sent to our servers:

- Your 16-digit account key, encrypted with the Windows DPAPI on Windows or a 0600 file on Unix-like systems
- Your selected protocol, DNS blocking categories, kill switch state, lockdown state, last-connected server, and other UI preferences
- Cached server list (last fetch was less than 30 seconds ago)
- Cached privacy and terms documents (last fetch was less than one hour ago)

You may inspect and delete these files at any time. Locations:

- Windows: `%LOCALAPPDATA%\fvpn\`
- Linux / macOS: `$HOME/.config/fvpn/`

### 3.2 On the VPN server

Each VPN server (Helsinki, Amsterdam, Virginia, Singapore at the time of writing) is hardened to disable persistent journald, rsyslog, and shell history. Per-protocol daemons are configured with `log: level: none`. The result is that no user-attributable data is recorded on the VPN server itself.

What is recorded:

- System uptime and load averages, written to in-memory `/proc` files by the Linux kernel (not persisted)
- Total bytes transferred on the network interface, also `/proc`-based and not persisted
- The fvpn-agent service samples both metrics every 30 seconds and reports them to the database, then forgets the previous sample

What is not recorded on the VPN server:

- Source IP addresses
- Destination IP addresses or hostnames
- DNS queries (dnscrypt-proxy is configured with `query_log: file: ''`)
- Bandwidth per user
- Session start or stop times

### 3.3 In the authentication database (Supabase)

The Supabase Postgres database stores the minimum information required to operate accounts and subscriptions:

| field | what | retention |
|-------|------|-----------|
| `keys.key_hash` | SHA-256 hex of your 16-digit key. Plaintext key never leaves your device. | While account exists |
| `keys.sub_until` | Subscription expiry timestamp. Null if no active subscription. | While account exists |
| `keys.created_at` | Timestamp of account creation. | While account exists |
| `devices.device_id` | Random 16-byte hex per device. Generated client-side. | While device is active + 30 days |
| `devices.name_words` | Three random words (e.g. `Hacker Anonymous Killer`). | Same as device_id |
| `devices.last_seen` | Timestamp of last heartbeat. | Same as device_id |
| `payments.tg_note` | Operator note linking a Telegram payment to a key hash. Plain text. | Same as account |
| `request_counts` | Sliding window rate-limit counters by IP or key hash. | Two hours rolling |
| `servers.*` | Public server metadata (region, city, public keys, ports, load). No user data. | Indefinite |

Server-side connection logs that Supabase generates as part of its managed Postgres service include the public IP address of the device making the API call. We have no operational control over these logs (they are part of Supabase's managed infrastructure). Supabase's privacy policy is at [supabase.com/privacy](https://supabase.com/privacy).

The roadmap for v1.5 includes optional onion routing of API calls through your own active VPN session, eliminating the IP address from any Supabase logs after the first connection. Until then, this is a known limitation and we disclose it here rather than promising what we cannot guarantee.

### 3.4 In Telegram

If you contact [@aimwork](https://t.me/aimwork) for payment confirmation or support, the conversation is stored on Telegram's infrastructure under their privacy policy. We retain only the operator note linked to your key hash (a short string the operator types to remember which payment matches which account). The full Telegram conversation is not exported to our database.

We recommend using a Telegram username that is not linked to your real identity if you want maximum pseudonymity.

## 4. What we do not collect

To remove ambiguity, we do not collect any of the following at any layer:

- Real names
- Email addresses
- Phone numbers
- Postal addresses
- Browsing history, URLs, or domains visited
- Files transferred
- Application telemetry (no crash reports, no analytics, no UI usage tracking)
- Cookies (the application is a native binary, there are no browser-based interactions)
- Marketing identifiers, advertising IDs, or device fingerprints

If you give us any of this voluntarily through Telegram support, we keep only what is necessary to resolve the request and delete it after thirty days unless you ask for the conversation to be retained.

## 5. How we use the information we do collect

The information described in section 3 is used solely for the following operational purposes:

- Authenticating your account when you launch the application
- Enforcing the maximum of three concurrent devices per account
- Serving the public server list to clients
- Routing rate limiting on registration and login attempts (to prevent brute force)
- Showing you the remaining days of your subscription
- Operator visibility into server health (load, online status) for capacity planning

We do not sell or share this information for any commercial purpose. We do not use it for advertising. We do not use it to build a profile of you across services.

## 6. Third-party processors

The following third parties process information on our behalf or on yours. Each has its own privacy policy, linked below.

| processor | role | data they see | their policy |
|-----------|------|---------------|--------------|
| Google Cloud Platform | Hosts our four VPN servers and the operator's billing account | VPN server uptime, system metrics, the operator's billing identity | [cloud.google.com/terms/cloud-privacy-notice](https://cloud.google.com/terms/cloud-privacy-notice) |
| Supabase | Hosts the authentication and subscription database | Key hashes, device IDs, server metrics, IP addresses of API callers | [supabase.com/privacy](https://supabase.com/privacy) |
| GitHub | Hosts source code and binary releases | Git commit metadata, binary download counts | [github.com/site/privacy](https://github.com/site/privacy) |
| Telegram | Carries operator support conversations | Whatever you write in the chat | [telegram.org/privacy](https://telegram.org/privacy) |
| Cloudflare | Free tier of the docs repo serving privacy and terms markdown over `raw.githubusercontent.com` | IP addresses of clients fetching the documents | [cloudflare.com/privacypolicy](https://www.cloudflare.com/privacypolicy/) |

## 7. International transfers

The Service is operated from the European Union. Data may be transferred to:

- Google Cloud regions where the VPN servers are hosted (currently Helsinki FI, Amsterdam NL, Ashburn US, Singapore SG)
- Supabase's primary region for the authentication database
- GitHub's US-based infrastructure for binary releases

Where applicable, transfers outside the European Economic Area rely on the European Commission's standard contractual clauses with the third-party processors.

## 8. Retention

| data | retention | what triggers deletion |
|------|-----------|------------------------|
| Account key hash | While account exists | Logout from any device, or 30-day inactivity after subscription expiry, or written request to operator |
| Device entries | While device is active + 30 days after last heartbeat | Manual logout removes entry immediately |
| Subscription history | While account exists, then 30 days | Account deletion |
| Server load metrics | 90 days rolling | Automatic cleanup |
| Rate limit counters | 2 hours rolling | Automatic cleanup |
| Operator support conversations | 30 days after resolution unless you ask otherwise | Manual deletion by operator |

To request deletion of your account and all associated data outside of these automatic windows, contact [@aimwork](https://t.me/aimwork) on Telegram with your key hash. Plaintext keys are not stored anywhere, so the operator cannot look up your account from your plaintext key alone; you must provide the SHA-256 hash, or the operator note your payment was associated with, or the device name your client displays.

## 9. Your rights under GDPR

If you are in the European Economic Area, the United Kingdom, or another jurisdiction with comparable rights, you have:

- **Right of access**: ask for a copy of all data tied to your account hash
- **Right to rectification**: ask for corrections to incorrect data (in practice we hold so little data that this rarely applies)
- **Right to erasure ("right to be forgotten")**: ask for full deletion as described in section 8
- **Right to restrict processing**: ask us to pause processing of your data while we resolve a dispute
- **Right to data portability**: ask for your data in a machine-readable format (JSON)
- **Right to object**: object to processing based on legitimate interest

Requests are typically resolved within seven days. There is no fee for the first request in any twelve-month period.

If you believe your rights have been violated, you may complain to your local data protection authority. In Germany, this is the Bundesbeauftragte für den Datenschutz und die Informationsfreiheit ([bfdi.bund.de](https://www.bfdi.bund.de/)).

## 10. Security measures

- VPN traffic is encrypted with the noise protocol (WireGuard family) or TLS 1.3 (Hysteria2, Reality)
- Account key is hashed with SHA-256 before any network transmission
- Account key is encrypted at rest on the device using Windows DPAPI (Windows) or POSIX file permissions 0600 (Linux, macOS)
- Database access from the client is limited by Row-Level Security policies; the public anon key cannot read or write any table directly, only call a small set of audited Postgres functions
- The operator's service-role key is stored only on the VPN servers (in chmod 600 environment files) and on the operator's local machine for incident response

We do not promise that no breach is possible. We promise to disclose any confirmed breach within 72 hours of confirmation, in accordance with GDPR Article 33.

## 11. Children

The Service is not directed at children under the age of 13. We do not knowingly collect data from children. If you are a parent or guardian and believe a child has registered an account, contact [@aimwork](https://t.me/aimwork) and the account will be deleted.

The operator of this Service is currently a minor. Operations are conducted with the consent of the operator's legal guardian. This may affect the legal recourse available against the Service in some jurisdictions. We disclose this to be transparent rather than to claim immunity.

## 12. Changes

We may update this Privacy Policy. The "Last updated" date at the top of this document will reflect the most recent revision. Material changes (changes to what is collected, who has access, or how long it is retained) will be announced in the project Telegram channel and in the application's home screen on next launch.

You are responsible for reviewing this policy from time to time. The client application fetches the latest version on every launch (with a one-hour cache).

## 13. Contact

For privacy-related questions, requests, or complaints:

- Telegram: [@aimwork](https://t.me/aimwork)
- GitHub Issues (public): [github.com/t3rmynal/fsociety-vpn/issues](https://github.com/t3rmynal/fsociety-vpn/issues)

For the most up-to-date version of this document:

- Markdown source: [github.com/t3rmynal/fsociety-vpn-docs/blob/main/privacy.md](https://github.com/t3rmynal/fsociety-vpn-docs/blob/main/privacy.md)
- Raw URL fetched by the client: [raw.githubusercontent.com/t3rmynal/fsociety-vpn-docs/main/privacy.md](https://raw.githubusercontent.com/t3rmynal/fsociety-vpn-docs/main/privacy.md)
