# 🔐 NowaraJS JWT

There are already plenty of JWT libraries out there. I built this one mostly for myself—to learn, to experiment, and to have something lightweight that fits my workflow without extra bloat.

## Why this package?

Honestly? **I just wanted to try building one.**

It wraps jose with sane defaults, handles expiration with human-readable strings like `"2 hours"`, and auto-manages claims so I don't have to think about `iat`, `nbf`, or `jti` every time. Nothing revolutionary, just convenient.

## 📌 Table of Contents

- [Features](#-features)
- [Installation](#-installation)
- [Usage](#-usage)
- [API Reference](#-api-reference)
- [License](#-license)
- [Contact](#-contact)

## ✨ Features

- ⏱️ **Human-Readable Expiration**: Write `"15 minutes"` or `"2 days"` instead of calculating seconds.
- 🔍 **UUID v7 for JTI**: Every token gets a unique, time-sortable JWT ID automatically.
- 📅 **Auto-Managed Claims**: `iat`, `nbf`, `exp`, `jti` are set by default—override only what you need.
- 🔒 **Built on Jose**: Rock-solid cryptography under the hood with HS256 signing.
- 🔑 **Secret Validation**: Enforces minimum 32-character secrets for HS256 security.
- ⚠️ **Typed Errors**: Throws `HttpError` (401) with specific error keys for expired, invalid, or malformed tokens.
- 📦 **Bun-Optimized**: Designed for Bun runtime, zero unnecessary dependencies.

## 🔧 Installation

```bash
bun add @nowarajs/jwt @nowarajs/error
```

## ⚙️ Usage

### Sign a Token

Use `signJWT` to create a token. The third argument accepts numbers, `Date` objects, or human-readable strings.

```ts
import { signJWT } from '@nowarajs/jwt';

// Secret must be at least 32 characters
const secret = 'your-secret-key-at-least-32-chars!';

const token = await signJWT(
  secret,
  { userId: '123', role: 'admin' },
  '2 hours'
);
```

### Verify a Token

Returns the decoded payload or throws `HttpError` (401) if invalid/expired.

```ts
import { verifyJWT } from '@nowarajs/jwt';

try {
  const result = await verifyJWT(token, secret);
  console.log('User ID:', result.payload.userId);
} catch (error) {
  // HttpError with specific error key
  console.log('Token verification failed:', error.message);
}
```

### Verify with Options

Validate issuer and audience claims:

```ts
import { verifyJWT } from '@nowarajs/jwt';

const result = await verifyJWT(token, secret, {
  issuer: 'Core-Issuer',
  audience: 'Core-Audience'
});
```

### Expiration Formats

```ts
// Seconds from now
await signJWT(secret, payload, 900);

// Date object
await signJWT(secret, payload, new Date('2026-12-31'));

// Human-readable (my favorite)
await signJWT(secret, payload, '15 minutes');
await signJWT(secret, payload, '1 week');
await signJWT(secret, payload, '30 days');
```

### Custom Claims

Override any default claim by including it in your payload:

```ts
const token = await signJWT(
  'secret',
  {
    userId: '123',
    iss: 'MyApp',           // Override issuer
    aud: ['web', 'mobile'], // Override audience
    sub: 'user-123'         // Override subject
  },
  '1 day'
);
```

## 📚 API Reference

Full docs: [nowarajs.github.io/jwt](https://nowarajs.github.io/jwt/)

## ⚖️ License

MIT - Feel free to use it.

## 📧 Contact

- Mail: [nowarajs@pm.me](mailto:nowarajs@pm.me)
- GitHub: [NowaraJS](https://github.com/NowaraJS)
