# The Project API

## API Top Layer

The Project will have only one API entry point /api/ and multiple subprojects and microservices will be connected to it.

Frontend will use only top-layer API entry point and all required methods from microservices will be available through it with API documentation and E2E API tests on the top-layer API.

## API self-documentation

API is self-documenting using Swagger UI.
Swagger UI is available at `/api/docs`.

Every controller, DTO and model should be documented in co using framework and language best practices.

## DTOs

Every controller should have its own *.dtos.%EXT% file or dtos folder with *.dtos.%EXT% files for requests and responses.

Every DTO should be documented with Swagger UI with description, examples and validation rules.

## Query parameters

Quesry parameters are DTOs.

## Validation

Validation should be done on DTOs layer using framework and language best practices.

## Auth

Every endpoint marked with [AUTH] requires "Authorization: Bearer <JWT_TOKEN>" header.

Every authorization method should return JWTAuthToken.

### JWTAuthToken

```json
{
    "access_token": "<JWT_TOKEN>",
    "token_type": "bearer",
    "expires_in": 1000000000000
}
```



### GET /api/v1/auth/mock_login?id=<USER_ID>

Important: enabled only in white-listed environments. Used for testing purposes.

Returns `JWTAuthToken`.

### GET /api/v1/auth/google/init

Returns a redirect to Google OAuth2 consent screen.

### REDIRECT /api/v1/auth/google/redirect

Redirects to Google OAuth2 consent screen, uses same method as /api/v1/auth/google/init.

### GET /api/v1/auth/google/callback?code=<CODE>&state=<STATE>

Validates the `code`, finds user by email or creates a new one and emits `registration_complete` or `login` event, returns `JWTAuthToken`.

### POST /api/v1/auth/register

Registers a new user with email and password, sets `email_verified` to false, emits `email_verification` event and returns `JWTAuthToken`.

### POST /api/v1/auth/login

Login with email and password, emits `login` event and returns `JWTAuthToken`.

### GET /api/v1/auth/verify_email?token=<TOKEN>

Verifies the email and sets `email_verified` to true, emits `registration_complete` event and returns `JWTAuthToken`.

## User

Fetchs and displays only required fields without sensitive and not needed data, configurable on ORM layer and specific endpoint layer.

### [AUTH] GET /api/v1/user/me

User response example:

```json
{
    "id": "<UUID>",
    "email": "<EMAIL>",
    "email_verified": true,
    "full_name": "<FULL_NAME>",
    "username": "<USERNAME>",
    "avatar_url": "<AVATAR_URL>",
    "is_active": true,
    "is_superuser": false,
    "created_at": "<DATETIME>",
    "updated_at": "<DATETIME>"
}
```

Fields:
- `avatar_url` - optional: full url or generated path to file (see [STORAGE.md](STORAGE.md))


### [AUTH] PATCH /api/v1/user/

Updates user information.

## Payments

Check [PAYMENT_SYSTEM_AND_MONETIZATION.md](./PAYMENT_SYSTEM_AND_MONETIZATION.md) for more information.

### Endpoints with points consumption [POINTS_CONSUMPTION]

Every endpoint with points consumption should have `spent_points` field in response and fail with 402 status code if user has not enough points < ENDPOINT_POINTS_COST_THRESHOLD


### [AUTH] POST /api/v1/payments/stripe/checkout-session

```json
{
    "price_id": "<PRICE_ID>",
    "reference_id": "<USER_ID>"
}
```

Important: 
Pass `user.id` as `reference_id` and RevenueCat `product_id` as `price_id` in `metadata` to Stripe API.

Returns `checkout_url`.

### GET /api/v1/payments/subscriptions

Returns the list of subscriptions (proxies/maps RevenueCat available subscriptions/products).

Important:
- OptionalAuth endpoint.
- If user is authorised, returns available subscriptions with mapped user statuses from RevenueCat.
- If user is not authorised, returns the same available subscriptions with `inactive` status.

```json
[
    {
        "id": "<SUBSCRIPTION_ID>",
        "product_id": "<PRODUCT_ID>",
        "status": "<STATUS>",
        "current_period_start": "<DATETIME>",
        "current_period_end": "<DATETIME>",
        "cancel_at_period_end": true
    }
]
``` 

### [AUTH] GET /api/v1/payments/subscriptions/active

Returns the list of active subscriptions for the current user (proxies/maps RevenueCat subscriptions).

```json
[
    {
        "id": "<SUBSCRIPTION_ID>",
        "product_id": "<PRODUCT_ID>",
        "status": "<STATUS>",
        "current_period_start": "<DATETIME>",
        "current_period_end": "<DATETIME>",
        "cancel_at_period_end": true
    }
]
```

### POST /api/v1/payments/revenuecat/webhook

```json
{
    "event": "SUBSCRIPTION_PURCHASED",
    "data": {
        "customer_id": "<CUSTOMER_ID>",
        "subscription_id": "<SUBSCRIPTION_ID>",
        "product_id": "<PRODUCT_ID>",
        "status": "<STATUS>",
        "current_period_start": "<DATETIME>",
        "current_period_end": "<DATETIME>",
        "cancel_at_period_end": true
    }
}
```

Emits `subscription_purchased` event.

## AI /api/v1/ai/*

API Layer over AI - requires auth, validates balance and subscription, deducts spent points [PAYMENT_SYSTEM_AND_MONETIZATION.md](PAYMENT_SYSTEM_AND_MONETIZATION.md), calls ai microservice

## [AUTH][PAYMENT_REQUIRED] POST /api/v1/ai/ask

Ask AI microservice

```json
{
   "query": "<QUERY>"
}
```

Response example:

```json
{
    "response": "<RESPONSE>",
    "spent_points": <POINTS>
}
```
