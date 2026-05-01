# Privacy Policy

Last updated: 2026-05-01

## Who we are

fsociety vpn is a small project run by a single individual, not a registered company. Contact: Telegram [@aimwork](https://t.me/aimwork). The project started as a private VPN for a circle of friends and may be opened up to a slightly wider audience over time.

## What we collect

We try to collect as little as possible. Concretely:

- A SHA256 hash of your 16 digit account key. The plaintext key never reaches the server, and we cannot recover it for you.
- A per device record: a random device id (16 random bytes, hex), a 3 word display name, and a `last_seen` timestamp. Used only to enforce the 3 device per key cap.
- Server load metrics from each VPS (CPU, memory, bandwidth aggregates), reported every 30 seconds by the server agent. Not tied to any user.

## What we do not collect

We do not run, and never have run, any of the following on the VPN servers:

- Connection logs (no syslog, no journald persistent storage, no rsyslog).
- DNS query logs.
- Browsing history or any record of destinations contacted through the tunnel.
- Traffic content of any kind.
- Email, phone number, real name, or any other identity attribute.

## Honest scope

The Supabase backend that handles registration, login, and heartbeat is reached over HTTPS directly from the client. That means the Supabase project sees the client IP address on every RPC call. We do not store that IP in any of our tables, but the underlying provider may keep short term request logs.

If you want to hide that IP, route the client through a different VPN before launching it, or wait for the v1.5 option that tunnels API calls through your own active fsociety connection.

## Retention

- Device records are kept while the subscription is active and for 30 days after expiry, then purged.
- Account key hashes are kept until you ask us to delete the account, or until 90 days after the subscription expires with no renewal, whichever comes first.
- Payment records (tier, amount, status) are kept while the subscription is active and for 12 months after, for accounting honesty.

## Third parties

- Google Cloud (VPS hosting for the VPN servers).
- Supabase (hosted Postgres holding key hashes and subscription state).
- GitHub (binary releases, this docs repository, and the source code).
- Cloudflare: not used in v1. If a Cloudflare Worker proxy ships in v1.5, it will be opt in and called out in this document.

Each of these providers can see the metadata they need to do their job, no more.

## Your rights

- Logout from the client removes your device record.
- A message to [@aimwork](https://t.me/aimwork) on Telegram with your account key hash will trigger full deletion of your key, devices, and payment history within 7 days.
- You can export your own data on request.

## Changes

If anything in this policy changes, the updated text lands in this file in the [fsociety-vpn-docs](https://github.com/t3rmynal/fsociety-vpn-docs) repository. The client fetches the latest version on every launch and shows the `Last updated` date on the privacy screen.

This policy reflects how the service actually runs today, not how we wish it ran. If a future change breaks any promise above, the old wording will stay in the git history of this repo for anyone to compare.
