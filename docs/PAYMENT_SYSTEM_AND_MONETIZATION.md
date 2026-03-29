# Payment System and Monetization

## PPA - Pay Per Action

We use PPA for cost control and monetization.

## Virtual Currency or Points System

We use RevenueCat for virtual currency until we have enough users to implement our own system:

https://www.revenuecat.com/docs/offerings/virtual-currency

User gets PRICE[SUBSCRIPTION_PRICE] * BUY_RATIO tokens for subscription every month.

It should be enough for average user to use the service without buying additional tokens.

Tokens could be hidden from user, could be shown as "credits" or "points".

User gets tokens for:
- registration
- **subscription**
- [promo actions]
- [daily missions]
- [assistance for community or other users]
- [transfered between users]

### Minimal price of action calculation

```
MIN_PRICE = FIXED_SIZE_OF_ACTION + (MODEL_1_TOKENS_USED * MODEL_1_TOKENS_PRICE + MODEL_2_TOKENS_USED * MODEL_2_TOKENS_PRICE + ... + MODEL_N_TOKENS_USED * MODEL_N_TOKENS_PRICE) * SPENT_RATIO 
```

### Key methods

- `get_balance()` - returns the current balance
- **secured** `add_balance(amount)` - adds the specified amount to the balance
- **secured** `deduct_balance(amount)` - deducts the specified amount from the balance
- `list_subscriptions()` - returns the list of subscriptions

## Subscription Plans

Setup subscription plans in RevenueCat:

- Register at [RevenueCat](https://app.revenuecat.com/)
- Connect Stripe account: `https://app.revenuecat.com/projects/<project_id>/new-app/stripe`
- [Connect App Store Connect account: `https://app.revenuecat.com/projects/<project_id>/new-app/app_store`]
- [Connect Google Play Console account: `https://app.revenuecat.com/projects/<project_id>/new-app/play_store`]
- Create project or use test project
- Create product with subscription type: `https://app.revenuecat.com/projects/<project_id>/product-catalog/products/new`
- Create virtual currency and attach it to subscription product: `https://app.revenuecat.com/projects/<project_id>/product-catalog/virtual-currencies/new-currency`
- Set up entitlements and attach to product: `https://app.revenuecat.com/projects/<project_id>/product-catalog/entitlements`
- Setup webhook for /api/v1/payments/revenuecat/webhook: `https://app.revenuecat.com/projects/<project_id>/settings/webhooks`


## Environments

We use test mode for RevenueCat and Stripe in local, test and dev environments.