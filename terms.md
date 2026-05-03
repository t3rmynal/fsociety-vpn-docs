# Terms of Service

**Last updated**: 2026-05-03 

These Terms of Service ("Terms") govern your use of fsociety vpn (the "Service"). By creating an account or otherwise using the Service, you agree to these Terms. If you do not agree, do not use the Service.

These Terms are written in plain English. Where there is genuine legal language, we explain it.

## 1. Definitions

- **Service** means the fsociety vpn application, the backend infrastructure that supports it, and the documentation in this repository.
- **Operator** means the individual responsible for running the Service, contactable through Telegram at [@aimwork](https://t.me/aimwork).
- **Account** means the 16-digit key the Service issues you, together with any subscription state and device entries linked to that key.
- **Device** means a single instance of the client application running under one Account.

## 2. Eligibility

You may use the Service if all of the following are true:

- You are at least 13 years of age, or older if your local jurisdiction requires
- You are not located in a jurisdiction subject to comprehensive embargo by the European Union or the United States
- Your intended use does not violate the Acceptable Use Policy in section 6

The Service makes reasonable effort to be available worldwide but is operated from the European Union and may not be lawful in every jurisdiction. You are responsible for determining whether using a VPN is permitted where you are.

## 3. Account creation

The Service does not collect personal information at registration. The application generates a 16-digit key locally on first launch, hashes it with SHA-256, and submits the hash to the backend. You are responsible for storing the plaintext key safely. **There is no recovery process. If you lose the key, the Account is unrecoverable.**

The application asks you to confirm you have saved the key, then asks you to paste it back to verify. Only after the verification step is the hash committed to the backend. If you abandon registration before the verification step, no Account is created.

## 4. Devices

Each Account is limited to three concurrent devices. Each device receives a 3-word identifier (e.g. `Hacker Anonymous Killer`) generated client-side. Adding a fourth device while three are active will be refused by the backend with the error `device_limit_exceeded`.

To free a device slot, log out from the application's Account screen on the device you want to remove. Logout removes the device entry from the backend immediately and clears the local keystore.

## 5. Subscriptions and payment

The Service offers two subscription tiers. Both are paid in US dollars (or equivalent in any commonly-traded currency, at the operator's discretion at time of confirmation).

| tier | duration | price |
|------|----------|-------|
| 1 month | 30 days | $4 USD |
| 3 months | 90 days | $10 USD |

Maximum single purchase is the 3-month tier. There is no annual or lifetime option in v0.1.

### 5.1 How to pay

1. From the home screen, press F3 (or open the Account screen and choose Subscribe). The application displays your key hash and the Telegram handle [@aimwork](https://t.me/aimwork).
2. Send the payment to the operator on Telegram with your key hash as the note. Acceptable payment methods are at the operator's current discretion (typically Telegram payments, USDT TRC-20, or other cryptocurrencies).
3. The operator confirms receipt and extends your subscription manually. This usually takes less than 24 hours during European business hours.

### 5.2 Refunds

The Service does not issue refunds for partial periods, accidental purchases, or change of mind. Pre-paid time is forfeit on Account deletion. Refunds for service outages exceeding 72 consecutive hours within a single subscription period may be granted at the operator's discretion as service credit, not as cash.

### 5.3 Auto-renewal

There is no auto-renewal. There is no card on file. There is no "subscription" in the recurring-billing sense. Each tier is a one-time purchase that extends the expiry date of your Account by the corresponding duration.

## 6. Acceptable Use Policy

Use of the Service must comply with applicable law and with these prohibitions. Violation may result in immediate termination of the Account without refund.

### 6.1 Strictly prohibited (zero tolerance)

- **Child sexual abuse material (CSAM)** of any kind. Suspected CSAM activity will be reported to the relevant authorities, your Account will be terminated, and the operator will cooperate with law enforcement in the operator's jurisdiction.
- **Targeted attacks**: stalking, harassment, doxxing, swatting, organized harassment campaigns, gendered or racial harassment, threats of violence directed at identifiable individuals.
- **Mass-scale spam**: distribution of unsolicited bulk messages by email, SMS, instant messaging, or other channels.
- **Distributed denial-of-service attacks** originating from or transiting through the Service.
- **Distribution of malware** including ransomware, spyware, stalkerware, banking trojans, cryptocurrency miners installed without consent, or worms.
- **Fraud against identifiable victims**: phishing of specific individuals, romance scams, business email compromise, advance-fee fraud, identity theft.

### 6.2 Restricted (may result in termination at operator discretion)

- **High-volume traffic** that materially impacts other users on the same server (sustained > 100 Mbps per device for extended periods may be throttled or terminated)
- **Mining cryptocurrency through the VPN** at a scale that consumes disproportionate server resources
- **Operating publicly-accessible services** through the VPN that attract attention from upstream providers (running a public Tor exit, hosting a public website, hosting BitTorrent trackers)
- **Reselling or sub-leasing** the Service to others without prior written agreement with the operator

### 6.3 Permitted

- **Bypassing geographic content restrictions** (streaming services, news sites, etc.)
- **Bypassing network-level censorship** in jurisdictions where this is lawful for you
- **Privacy from your local ISP**, employer's network, or coffee shop Wi-Fi
- **Personal BitTorrent use** for non-infringing material (the Service does not block BitTorrent traffic; copyright holders may still pursue you for infringement)
- **Anonymous browsing**, social media use, communication
- **Anything else not prohibited above** that complies with law in the jurisdictions you transit

## 7. Service availability

The Service is provided on a best-effort basis. The operator does not commit to a specific service-level agreement. Outages, planned maintenance windows, and infrastructure failures are expected as a normal part of operating a software service.

The operator commits to the following soft targets:

- 99% monthly availability of the authentication backend
- 95% monthly availability per VPN server (degraded service when one server is down can usually be replaced with another from the rotation)
- Critical security patches deployed within 72 hours of disclosure
- Subscription credit for outages exceeding 72 consecutive hours within a billing period (see section 5.2)

These targets are aspirational and not contractually guaranteed.

## 8. Termination

### 8.1 Termination by you

You may terminate your Account at any time by logging out from all devices through the Account screen, or by contacting [@aimwork](https://t.me/aimwork) and asking for full deletion. Pre-paid time is forfeit.

### 8.2 Termination by the operator

The operator may terminate your Account immediately and without refund for:

- Material violation of the Acceptable Use Policy in section 6
- Chargebacks or payment disputes initiated against the operator after the Service was provided in good faith
- Repeated abuse of the support channel
- Activity that exposes the operator to material legal risk

The operator may also terminate the entire Service with thirty days' written notice in the project Telegram channel. In that event, active subscriptions will receive a pro-rated credit if a successor service is available, or a refund of unused time at the operator's discretion.

## 9. Disclaimer of warranties

THE SERVICE IS PROVIDED "AS IS" AND "AS AVAILABLE" WITHOUT WARRANTIES OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO IMPLIED WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE, AND NON-INFRINGEMENT. THE OPERATOR DOES NOT WARRANT THAT THE SERVICE WILL BE UNINTERRUPTED, SECURE, OR ERROR-FREE.

The Service uses cryptographic primitives from upstream open-source projects (`golang.zx2c4.com/wireguard`, `xtls/xray-core`, `apernet/hysteria`, `charmbracelet/bubbletea`, `charmbracelet/lipgloss`, `charmbracelet/glamour`). The operator does not warrant the security of these upstream components beyond reasonable diligence in keeping them current.

## 10. Limitation of liability

TO THE MAXIMUM EXTENT PERMITTED BY APPLICABLE LAW, THE OPERATOR SHALL NOT BE LIABLE FOR ANY INDIRECT, INCIDENTAL, SPECIAL, CONSEQUENTIAL, OR PUNITIVE DAMAGES ARISING FROM YOUR USE OR INABILITY TO USE THE SERVICE.

THE OPERATOR'S TOTAL CUMULATIVE LIABILITY UNDER THESE TERMS, FOR ANY CAUSE WHATSOEVER, SHALL NOT EXCEED THE AMOUNT YOU HAVE PAID THE OPERATOR IN THE TWELVE MONTHS PRECEDING THE EVENT GIVING RISE TO LIABILITY. FOR NON-PAYING USERS, THE OPERATOR'S MAXIMUM LIABILITY IS ZERO.

These limitations apply even if the operator has been advised of the possibility of such damages, and even if a remedy fails of its essential purpose. Some jurisdictions do not allow the limitation of liability for consequential damages; in those jurisdictions, the operator's liability is limited to the maximum extent permitted by law.

## 11. Indemnification

You agree to indemnify and hold harmless the operator from any claim, demand, loss, damage, cost, or expense (including reasonable legal fees) arising from your use of the Service in violation of these Terms or in violation of applicable law.

## 12. Governing law and dispute resolution

These Terms are governed by the laws of the Federal Republic of Germany without regard to conflict-of-laws principles. The exclusive forum for disputes is the courts of Berlin, Germany. Where the consumer protection laws of your jurisdiction grant you a non-waivable right to bring proceedings in your local courts, this clause does not override that right.

For consumer disputes within the European Union, the European Commission's online dispute resolution platform is available at [ec.europa.eu/consumers/odr](https://ec.europa.eu/consumers/odr). The operator is not currently registered with this platform but is willing to engage informally to resolve disputes before they escalate.

## 13. Force majeure

Neither party is liable for delay or failure to perform an obligation under these Terms to the extent the delay or failure is caused by events beyond reasonable control, including but not limited to natural disasters, war, terrorism, strikes, supplier failures, government action, internet backbone outages, or pandemics.

## 14. Severability

If any provision of these Terms is held by a court of competent jurisdiction to be invalid, illegal, or unenforceable, the remaining provisions remain in full force and effect, and the invalid provision shall be reformed to the minimum extent necessary to make it valid and enforceable.

## 15. No waiver

The failure of the operator to enforce any provision of these Terms is not a waiver of the right to enforce that or any other provision in the future.

## 16. Entire agreement

These Terms, together with the [Privacy Policy](privacy.md), constitute the entire agreement between you and the operator concerning the Service, and supersede all prior agreements, whether written or oral.

## 17. Changes to these Terms

The operator may update these Terms. The "Last updated" date at the top of this document will reflect the most recent revision. Material changes (changes to the Acceptable Use Policy, pricing, refund policy, or limitation of liability) will be announced in the project Telegram channel and in the application's home screen on next launch.

Continued use of the Service after the effective date of an update constitutes acceptance of the updated Terms. If you do not agree to the updated Terms, you must terminate your Account before continuing to use the Service.

## 18. Contact

For questions about these Terms:

- Telegram: [@aimwork](https://t.me/aimwork)
- GitHub Issues (public): [github.com/t3rmynal/fsociety-vpn/issues](https://github.com/t3rmynal/fsociety-vpn/issues)

For the most up-to-date version of this document:

- Markdown source: [github.com/t3rmynal/fsociety-vpn-docs/blob/main/terms.md](https://github.com/t3rmynal/fsociety-vpn-docs/blob/main/terms.md)
- Raw URL fetched by the client: [raw.githubusercontent.com/t3rmynal/fsociety-vpn-docs/main/terms.md](https://raw.githubusercontent.com/t3rmynal/fsociety-vpn-docs/main/terms.md)
