# v3 ch. 9: Architecture and deployment profiles

- **Topic:** Where each part runs, what data goes where, and how users reach it.
- **Inputs:** synthesis §7–8 (privacy options); RS5 §3–6 (hosting, Cloudflare TLS termination, AWS Tokyo, identity); RS3 §7 (speech/privacy stacks); RS6 §4b (contracts, MHLW v7.0); RS7 §1 (juniors study after hours, mostly at home).

## Profiles

Portable pipeline: one codebase, profile set per deployment.

| Profile | Where | Data | When |
|---|---|---|---|
| **P-dev** | ARM64 laptop (Linux/WSL2) + 48 GB laptop as heavy-job worker | 🧪 synthetic only | S0 → |
| **P-cloud-JP synthetic** | AWS Tokyo, synthetic account; Cloudflare Access allowed (CEO continuity, demos) | 🧪 | S0 demo |
| **P-cloud-JP real** | AWS Tokyo, **separate real-data account**, region-locked to ap-northeast-1/3 by SCP | 🏥 per hospital agreement | S1 → |
| **P-browser-edge** (fallback) | Browser on the hospital PC: local video, in-browser clip cutting + masking (WebCodecs) | Only masked clips leave | If a hospital refuses full-video upload |
| **P-hospital-box** | Windows service + local models | Everything stays in hospital | Plan only (ch. 8) |

## Choices

- **Archive pilot video: upload the full video to S3 Tokyo (chosen).**
  - Server-side B02 cuts clips, applies overlay masks and checks out-of-body frames.
  - Juniors stream clips anywhere via signed URLs.
  - *Options considered:* only short masked clips leave (Claude's pick) · nothing leaves (in-hospital playback only).
  - **Dependency:** this needs the hospital's written approval (surgical video linked to a patient is 要配慮個人情報, sensitive personal information under APPI). If refused, switch that pilot to **P-browser-edge** without changing the rest of the system.
  - **Consequences:** a 委託 / joint-research contract covering video; SSE-KMS encryption; S3 access logging; retention per the hospital's rules; resumable multipart upload direct from the browser to S3 Tokyo (pre-signed URLs, integrity hash); 1–4 GB per case; hospital upload bandwidth to be checked. The surgeon can start narrating from the local file while the upload runs.
- **Real-data front door: direct to AWS Tokyo (chosen).**
  - Hospital login (OIDC; Cognito Tokyo as a bridge if the hospital has no IdP) + a Tokyo load balancer.
  - API and media never pass through a global CDN.
  - Static app assets (no patient data) may use a CDN.
  - The same `app.myrobots.jp` domain is kept via DNS.
  - Cloudflare Access stays for dev/synthetic and demos.
  - *Option considered:* Cloudflare Access for everything, which needs Enterprise "Regional Services (Japan)" plus proof that it covers Access (RS5 #11).

## Always-on rules

- Separate AWS accounts for synthetic and real data.
- The real-data account is locked to Tokyo/Osaka by SCP.
- Bedrock is limited to `jp.` / In-Region routes by IAM.
- Media goes direct to S3 Tokyo, never through a CDN.
- Terraform for AWS, Cloudflare and GitHub.
- Backups and PITR, with a copy to Osaka (DR stays in Japan).

**Decided by:** Karthi · 4 Oct 2026


> ⚠ **Amended 6 Oct 2026:** see `2026-10-06-v3-mvp-local-and-gateway.md`. The MVP runs on the two laptops only, and all AI goes through the gateway with switchable profiles (`dev-cloud` · `local` · `pilot-japan`).
