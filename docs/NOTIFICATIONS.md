# Notifications

For we use:
- OneSignal for push notifications
- Mandrill (MailChimp) for transactional e-mails: or any equivalent provider (e.g., OneSignal or a custom solution). Implementation is straightforward, so I’d leave the final choice to the business.

## OneSignal:

Trade-off: saves development time, but requires user to install OneSignal SDK.

<ignore>
Expensive alternative: 
- send push notifications using FCM, APNS and Web Push directly
- extract and save raw device_id, device_type, data from Firebase / APNS / Web Push SDKs separately
- send push notifications using FCM, APNS and Web Push directly
</ignore>

### [AUTH] GET /api/v1/notifications/onesignal/auth-hash

```python
import hmac
import hashlib

def generate_onesignal_hash(user_id: str, api_secret: str) -> str:
    """
    Calculates the SHA256 HMAC signature for OneSignal user authentication.
    """
    # 1. Convert secret and message to bytes (UTF-8)
    secret_bytes = api_secret.encode('utf-8')
    data_bytes = user_id.encode('utf-8')
    
    # 2. Create the HMAC signature
    signature = hmac.new(
        key=secret_bytes,
        msg=data_bytes,
        digestmod=hashlib.sha256
    ).hexdigest()
    
    return signature

# Example:
# auth_data = {
#     "hash": generate_onesignal_hash(str(user.id), os.getenv("ONESIGNAL_KEY")),
#     "external_id": str(user.id)
# }
```

```typescript
import * as crypto from 'crypto';

/**
 * Generates an HMAC-SHA256 hash for OneSignal Identity Verification.
 * @param userId - The unique identifier of the user in your database.
 * @param apiSecret - Your OneSignal REST API Key (keep this secret!).
 */
function getOneSignalHash(userId: string, apiSecret: string) {
    // 1. Create HMAC instance using SHA-256 and the secret key
    const hmac = crypto.createHmac('sha256', apiSecret);
    
    // 2. Update HMAC with the user's ID (must be a string)
    hmac.update(userId);
    
    // 3. Return the hex-encoded signature
    return hmac.digest('hex');
}

// Controller usage example:
// const hash = getOneSignalHash(user.id.toString(), process.env.ONESIGNAL_REST_API_KEY);
```

```rust
use hmac::{Hmac, Mac};
use sha2::Sha256;
use hex;

// Type alias for HMAC-SHA256
type HmacSha256 = Hmac<Sha256>;

/**
 * Computes the HMAC-SHA256 signature for secure OneSignal login.
 */
pub fn generate_onesignal_hash(user_id: &str, secret: &str) -> String {
    // 1. Initialize HMAC with the secret key slice
    let mut mac = HmacSha256::new_from_slice(secret.as_bytes())
        .expect("HMAC can accept keys of any size");
    
    // 2. Provide the data to be signed (user_id)
    mac.update(user_id.as_bytes());
    
    // 3. Finalize and encode to hexadecimal string
    let result = mac.finalize();
    hex::encode(result.into_bytes())
}
```

## Event-driven architecture

E-mail and push notification senders should be subscribed to events from message broker.

Example event:
- `subscription_paid` - send push notification to user with message "Your subscription has been paid" and e-mail notification to user with message "Your subscription has been paid"

- `email_verification` - send e-mail notification to user with message "Your e-mail verification code is: [CODE]"