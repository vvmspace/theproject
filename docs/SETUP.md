# A Guide to System Initialisation and Deployment

Welcome, esteemed developer, to the manual of commencement for this most sophisticated AI-First system. To ensure a seamless entry into our modular world, we have streamlined the setup process to be as elegant and straightforward as possible.

## The Principle of Unified Command

In keeping with our philosophy of efficiency, the entire system is designed to be brought to life through a single, authoritative command. This ensures that all components—from our NestJS API and FastAPI intellect to our high-performance Rust modules—awaken in perfect harmony.

### 1. The Universal Awakening

To launch every service within the monolithic monorepo simultaneously, one simply needs to execute the following command from the root of the repository:

```bash
# Bring the entire system to life
docker-compose up -d
```

This command orchestrates the creation of all necessary containers, networks, and volumes, leaving the system in a state of ready poise.

Containers are based on alpine linux.

### 2. Selective Commencement

Should you wish only to awaken specific domains for focused development or testing, you may do so by appending the names of the desired services to the command. For example, to launch only the API and the AI inference engine:

```bash
# Awaken only the chosen services
docker-compose up -d api ai
```

This selective approach allows for a more prudent use of system resources during targeted endeavours.

## Environment Preparation

Before one can issue these commands, it is essential that the local environment be prepared with the necessary `.env` files. Please refer to our [Environments Guide](docs/ENVIRONMENTS.md) for detailed instructions on the required variables and their preferred configurations.

## Where To Get Every Key

This section maps every external credential used by the repo to its source.

### Generated Locally, Not From a Vendor Dashboard

These are created by you and stored in env files:

- `JWT_SECRET`: generate your own random secret, for example with `openssl rand -hex 32`
- `AI_INTERNAL_API_KEY`: generate your own shared secret between `api` and `ai`
- `INTERNAL_API_KEY`: same value as `AI_INTERNAL_API_KEY` for the `ai` service
- `WHITELIST_ENVIRONMENTS`: your own env list, not a provider key
- `EMAIL_VERIFICATION_BASE_URL`: your own URL, not a provider key
- `STRIPE_SUCCESS_URL`: your own frontend/backend success URL
- `STRIPE_CANCEL_URL`: your own frontend/backend cancel URL
- `GRAFANA_ADMIN_USER`, `GRAFANA_ADMIN_PASSWORD`: local Grafana credentials you choose
- `MINIO_ROOT_USER`, `MINIO_ROOT_PASSWORD`, `MINIO_ACCESS_KEY`, `MINIO_SECRET_KEY`: local MinIO credentials you choose
- `POSTGRES_USER`, `POSTGRES_PASSWORD`, `POSTGRES_DB`, `DATABASE_URL`: local or infrastructure database credentials you choose
- `NATS_URL`, `AI_SERVICE_URL`: internal service URLs, not vendor secrets

### Google OAuth

Variables:

- `GOOGLE_CLIENT_ID`
- `GOOGLE_CLIENT_SECRET`
- `GOOGLE_CALLBACK_URL`

Official references:

- [Sign in with Google: OpenID Connect](https://developers.google.com/accounts/docs/OAuth2Login)
- [Create Google credentials](https://developers.google.com/workspace/guides/create-credentials)

Where to get them:

1. Open Google Cloud Console.
2. Go to `Google Auth platform` or `APIs & Services -> Credentials`.
3. Create an `OAuth client ID`.
4. Choose `Web application`.
5. Add your redirect URI:
    - local example: `http://localhost:3000/api/v1/auth/google/callback`
    - production example: `https://api.yourdomain.com/api/v1/auth/google/callback`
6. Copy the generated `Client ID` and `Client Secret`.

Put them into env:

```env
GOOGLE_CLIENT_ID=your-google-client-id
GOOGLE_CLIENT_SECRET=your-google-client-secret
GOOGLE_CALLBACK_URL=http://localhost:3000/api/v1/auth/google/callback
```

### Stripe

Variables:

- `STRIPE_SECRET_KEY`
- `STRIPE_WEBHOOK_SECRET`
- `STRIPE_SUCCESS_URL`
- `STRIPE_CANCEL_URL`

Official references:

- [Stripe API keys](https://docs.stripe.com/keys)
- [Stripe webhooks](https://docs.stripe.com/webhooks)

Where to get them:

1. Open the Stripe Dashboard.
2. In `Developers -> API keys`, copy your secret key:
    - test starts with `sk_test_`
    - live starts with `sk_live_`
3. In `Developers -> Webhooks`, create or open your webhook endpoint.
4. Reveal the endpoint signing secret:
    - it usually starts with `whsec_`
5. Decide where Stripe should redirect the browser after checkout:
    - `STRIPE_SUCCESS_URL`
    - `STRIPE_CANCEL_URL`

Put them into env:

```env
STRIPE_SECRET_KEY=sk_test_...
STRIPE_WEBHOOK_SECRET=whsec_...
STRIPE_SUCCESS_URL=http://localhost:3000/payments/success
STRIPE_CANCEL_URL=http://localhost:3000/payments/cancel
```

Important:

- this backend uses only the server-side secret key
- if you later add frontend Stripe Elements or client-side flows, you will also need Stripe publishable keys, but the current repo does not use them

### RevenueCat

Variables:

- `REVENUECAT_API_KEY`
- `REVENUECAT_WEBHOOK_SECRET`
- `REVENUECAT_AVAILABLE_PRODUCTS`

Official references:

- [RevenueCat API keys](https://www.revenuecat.com/docs/projects/authentication)
- [RevenueCat webhooks](https://www.revenuecat.com/docs/integrations/webhooks)

Where to get them:

1. Open the RevenueCat dashboard.
2. Open your project.
3. Go to `Project settings -> API keys`.
4. Create or copy a `Secret API key` for backend use.
5. Go to `Integrations -> Webhooks`.
6. Add your webhook endpoint URL:
    - local tunnel example: `https://your-ngrok-url/api/v1/payments/revenuecat/webhook`
    - production example: `https://api.yourdomain.com/api/v1/payments/revenuecat/webhook`
7. Set an authorisation header value in RevenueCat for webhook calls and reuse that value as `REVENUECAT_WEBHOOK_SECRET`.
8. Add your product identifiers to `REVENUECAT_AVAILABLE_PRODUCTS`, comma-separated.

Put them into env:

```env
REVENUECAT_API_KEY=rcb_...or_your_secret_key
REVENUECAT_WEBHOOK_SECRET=your-shared-webhook-secret
REVENUECAT_AVAILABLE_PRODUCTS=pro_monthly,pro_yearly
```

Important:

- use a RevenueCat secret server key on the backend
- do not expose secret keys in the app or browser
- RevenueCat also has public SDK keys for mobile/web clients, but this repo currently reads only the backend secret key

### OpenAI

Variables:

- `OPENAI_API_KEY`

Official references:

- [OpenAI quickstart authentication](https://platform.openai.com/docs/quickstart/authentication)
- [How to create and use an OpenAI API key](https://help.openai.com/en/articles/4936850-how-to-create-and-use-an-api-key)

Where to get it:

1. Open the OpenAI platform dashboard.
2. Create an API key on the API keys page.
3. Copy it once and store it securely.

Put it into env:

```env
OPENAI_API_KEY=sk-...
```

### Gemini

Variables:

- `GEMINI_API_KEY`

Official references:

- [Using Gemini API keys](https://ai.google.dev/gemini-api/docs/api-key)
- [Google AI Studio](https://ai.google.dev/aistudio)

Where to get it:

1. Open Google AI Studio.
2. Open `Projects` if needed and select or import a Google Cloud project.
3. Open `API Keys`.
4. Create a Gemini API key.

Put it into env:

```env
GEMINI_API_KEY=your-gemini-key
```

Notes:

- the project supports multiple Gemini keys separated by commas for rotation
- example: `GEMINI_API_KEY=key1,key2,key3`

### OpenRouter

Variables:

- `OPENROUTER_API_KEY`

Official references:

- [OpenRouter API keys](https://openrouter.ai/docs/api-keys)

Where to get it:

1. Open the OpenRouter dashboard.
2. Create an API key.
3. Optionally set a credit limit on the key.

Put it into env:

```env
OPENROUTER_API_KEY=sk-or-v1-...
```

Notes:

- the project supports multiple OpenRouter keys separated by commas

### OneSignal

Variables:

- `ONESIGNAL_APP_ID`
- `ONESIGNAL_REST_API_KEY`

Official references:

- [OneSignal quick-start API guide](https://documentation.onesignal.com/reference/quick-start-api-guide)

Where to get them:

1. Create or open your app in OneSignal.
2. Configure the target platform.
3. In the OneSignal dashboard, open app settings and copy:
    - `App ID`
    - `REST API Key`

### Mailchimp Transactional / Mandrill

Variables:

- `MANDRILL_API_KEY`
- `MANDRILL_FROM_EMAIL`
- `MANDRILL_FROM_NAME`

Official references:

- [Mailchimp Transactional quick start](https://mailchimp.com/developer/transactional/guides/quick-start/)
- [Mailchimp Transactional API reference](https://mailchimp.com/developer/transactional/api/)
- [Mailchimp email authentication](https://mailchimp.com/help/about-email-authentication/)

Where to get them:

1. Open Mailchimp Transactional.
2. Go to settings and create a new API key.
3. Verify the sender e-mail or sending domain you want to use.
4. Set `MANDRILL_FROM_EMAIL` to that verified sender.

### MinIO

Variables:

- `MINIO_ROOT_USER`
- `MINIO_ROOT_PASSWORD`
- `MINIO_ACCESS_KEY`
- `MINIO_SECRET_KEY`
- `MINIO_BUCKET`

Where to get them:

- for local Docker setup, you choose them yourself
- if you later switch from local MinIO to a managed S3-compatible storage, those credentials will come from that provider instead

### Quick Copy Checklist

If you want the shortest possible checklist, these are the keys most teams actually need to collect from vendor dashboards:

- Google: `GOOGLE_CLIENT_ID`, `GOOGLE_CLIENT_SECRET`
- Stripe: `STRIPE_SECRET_KEY`, `STRIPE_WEBHOOK_SECRET`
- RevenueCat: `REVENUECAT_API_KEY`, `REVENUECAT_WEBHOOK_SECRET`
- OpenAI: `OPENAI_API_KEY`
- Gemini: `GEMINI_API_KEY`
- OpenRouter: `OPENROUTER_API_KEY`
- OneSignal: `ONESIGNAL_APP_ID`, `ONESIGNAL_REST_API_KEY`
- Mandrill: `MANDRILL_API_KEY`

## Notifications

The `api` service now includes:

- `GET /api/v1/notifications/onesignal/auth-hash`
- event-driven notification processing for:
    - `auth.email_verification`
    - `auth.login`
    - `auth.registration_complete`
    - `payment.subscription_purchased`

Current behaviour:

- on registration: sends email verification email
- on login: sends push + e-mail notification
- on successful email verification: sends push + e-mail welcome notification
- on `payment.subscription_purchased`: sends push + e-mail payment confirmation

### Notification Env Variables

Add these values to root `.env` and, if you run NestJS directly, to [`api/.env`](api/.env):

```env
ONESIGNAL_APP_ID=
ONESIGNAL_REST_API_KEY=
MANDRILL_API_KEY=
MANDRILL_FROM_EMAIL=hello@example.com
MANDRILL_FROM_NAME=The Project
EMAIL_VERIFICATION_BASE_URL=http://localhost:3000/api/v1/auth/verify_email
```

### OneSignal Setup

What this backend uses from OneSignal:

- `ONESIGNAL_APP_ID`
- `ONESIGNAL_REST_API_KEY`

Official references:

- [OneSignal Keys & IDs](https://documentation.onesignal.com/docs/en/keys-and-ids)
- [OneSignal quick-start API guide](https://documentation.onesignal.com/reference/quick-start-api-guide)

Where to get them:

1. Create or open your app in OneSignal.
2. Configure the platform you actually need:
    - Web Push for browser notifications
    - iOS for APNS
    - Android for FCM
3. In the OneSignal dashboard open your app settings and copy:
    - `App ID`
    - `REST API Key`
4. Put them into `.env` as:

```env
ONESIGNAL_APP_ID=your-app-id
ONESIGNAL_REST_API_KEY=your-rest-api-key
```

Backend usage:

- the server sends push notifications through OneSignal REST API
- the server targets users by `external_id = user.id`
- the protected endpoint `GET /api/v1/notifications/onesignal/auth-hash` returns:
    - `app_id`
    - `external_id`
    - `hash`

Example response:

```json
{
  "app_id": "01234567-89ab-cdef-0123-456789abcdef",
  "external_id": "2d1f8f6b-4e41-4e59-b2ff-63b8f6188de2",
  "hash": "7d4f2f7e6f8d70b6f0db7e8f3d4ce5ad0d7747f1309a2e5f5f9b1cb3361a8a12"
}
```

Frontend integration example:

```ts
const token = '<JWT_TOKEN>';

const authRes = await fetch('http://localhost:3000/api/v1/notifications/onesignal/auth-hash', {
  headers: {
    Authorization: `Bearer ${token}`,
  },
});

const auth = await authRes.json();

await OneSignal.init({
  appId: auth.app_id,
});

await OneSignal.login(auth.external_id);
```

Important:

- this project follows the contract from [`docs/NOTIFICATIONS.md`](docs/NOTIFICATIONS.md) and exposes the `auth-hash` endpoint
- the latest OneSignal identity-verification flow may use JWT-based verification on the client/platform side, but the current project contract remains HMAC hash based
- if you use web push, you must also finish the site setup inside OneSignal and install their SDK in the frontend app

### Mandrill / Mailchimp Transactional Setup

What this backend uses from Mandrill:

- `MANDRILL_API_KEY`
- `MANDRILL_FROM_EMAIL`
- `MANDRILL_FROM_NAME`

Official references:

- [Mailchimp Transactional API quick start](https://mailchimp.com/developer/transactional/guides/quick-start/)
- [Mailchimp Transactional API reference](https://mailchimp.com/developer/transactional/api/)
- [Mailchimp email domain authentication](https://mailchimp.com/help/about-email-authentication/)

Where to get them:

1. Open Mailchimp Transactional / Mandrill.
2. Create an API key.
3. Verify the sender identity or domain you want to send from.
4. Put the values into `.env`:

```env
MANDRILL_API_KEY=your-api-key
MANDRILL_FROM_EMAIL=hello@yourdomain.com
MANDRILL_FROM_NAME=Your Product Name
```

Backend usage:

- the server sends email via `https://mandrillapp.com/api/1.0/messages/send`
- verification emails use `EMAIL_VERIFICATION_BASE_URL`

Example:

```env
EMAIL_VERIFICATION_BASE_URL=https://api.yourdomain.com/api/v1/auth/verify_email
```

If your frontend has a dedicated verify-email page, point this variable there instead, for example:

```env
EMAIL_VERIFICATION_BASE_URL=https://app.yourdomain.com/verify-email
```

Then make that page forward the token to the backend endpoint.

## How To Verify Notifications Locally

1. Fill the notification env variables.
2. Start the stack with `docker-compose up -d`.
3. Register a new user through Swagger or the API.
4. Confirm that:
    - a Mandrill email send attempt happens on `auth.email_verification`
    - `GET /api/v1/notifications/onesignal/auth-hash` returns data for an authorised user
5. Login with the same user.
6. Confirm that login triggers:
    - OneSignal push send attempt
    - Mandrill e-mail send attempt
7. Trigger RevenueCat webhook with `SUBSCRIPTION_PURCHASED` and confirm the purchase notification path.

## Useful Commands

Start everything:

```bash
cd docker
docker-compose up -d
```

Start only API + AI:

```bash
cd docker
docker-compose up -d api ai
```

Run API e2e tests through Docker profile:

```bash
cd docker
docker-compose -f docker-compose.test.yml up --build --abort-on-container-exit e2e-tests
```

Reset local artefacts:

```bash
./scripts/reset
```

## Troubleshooting and Maintenance

Should the system require a complete reset or a "pruning of the hedges," our `scripts/reset` utility is at your disposal for clearing temporary directories and build artifacts. For more persistent issues, pray, consult our [Support Documentation](docs/PROJECT_CONTEXT.md).

May your deployment be swift and your system remain ever splendid.
