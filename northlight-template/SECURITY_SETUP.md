# Account security setup

The new account features are implemented, but two third-party services must be
configured before deploying the site publicly.

## 1. Human verification (Cloudflare Turnstile)

Create a Turnstile widget at Cloudflare, add your production domain, then set:

```powershell
$env:NL_TURNSTILE_SITE_KEY = "your-public-site-key"
$env:NL_TURNSTILE_SECRET = "your-secret-key"
```

The browser receives only the site key. The server verifies each sign-up token
with Cloudflare. Until both values are set, the old checkbox is only a local
development convenience and is not bot protection.

## 2. Password-reset email with Resend

Verify a sending domain in Resend, then set:

```powershell
$env:NL_RESEND_API_KEY = "re_your_resend_api_key"
$env:NL_RESEND_FROM = "Northlight <support@your-domain.example>"
$env:NL_PUBLIC_BASE_URL = "https://your-domain.example"
```

Then start the server with HTTPS enabled in production:

```powershell
$env:NL_HTTPS = "1"
$env:NL_STRICT_HEADERS = "1"
python server/app.py
```

Do not commit any of these values to the repository. `NL_RESEND_FROM` must be
an address from a domain verified in your Resend account. SMTP is still
supported as a fallback. Reset links expire after
one hour, can be used only once, and sign out all existing sessions when used.
