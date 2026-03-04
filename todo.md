# TODO

## Paid API access — increase rate limits
- Switch Mistral API from free tier to pay-as-you-go on [La Plateforme](https://console.mistral.ai/)
- Free tier is rate-limited (~1 req/s) and not suitable for concurrent student use
- Pay-as-you-go removes monthly token caps and raises limits to ~500 req/min
- Cost is negligible for a university course (few €/month at current usage)

## GDPR compliance — consent at signup
- The deployment collects personal data: **email, name, and full chat history**
- Users must give informed consent before accessing the service
- Built-in Open WebUI consent screen is an enterprise-only feature — not available in the free self-hosted version
- Options:
  - [ ] Check with FS/UL DPO whether existing institutional IT consent covers this tool
  - [ ] If not: implement a consent gate via reverse proxy (Nginx) before Open WebUI
  - [ ] Or: collect consent manually (e.g. university form) before granting access
- Mistral API is EU-hosted and GDPR-compliant (not subject to US CLOUD Act) — data processing side is covered

## Access restricted to FS students and educators
- Integrate Microsoft Entra ID (Azure AD) OAuth so only `@fs.uni-lj.si` accounts can log in
- Steps:
  - [ ] Register app in [Azure Portal → Entra ID → App Registrations](https://portal.azure.com/)
  - [ ] Set supported account types to *"this organizational directory only"*
  - [ ] Add `email` as optional claim under Token Configuration → ID token
  - [ ] Set redirect URI to `https://your-server/oauth/oidc/callback`
  - [ ] Add env vars to Open WebUI: `MICROSOFT_CLIENT_ID`, `MICROSOFT_CLIENT_SECRET`, `MICROSOFT_CLIENT_TENANT_ID`, `ENABLE_OAUTH_SIGNUP=true`
  - [ ] Confirm with university IT that app registration is permitted or request it through them
- This also serves as a partial GDPR solution: consent is handled by Microsoft's OAuth flow for institutional accounts
