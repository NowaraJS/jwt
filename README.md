# 🔐 NowaraJS - JWT

## 📌 Table of Contents

- [🔐 NowaraJS - JWT](#-nowarajs---jwt)
	- [📌 Table of Contents](#-table-of-contents)
	- [📝 Description](#-description)
	- [🔧 Installation](#-installation)
	- [⚙️ Usage](#-usage)
		- [Basic JWT Operations](#basic-jwt-operations)
		- [Advanced Usage with Custom Claims](#advanced-usage-with-custom-claims)
		- [Human-Readable Expiration Times](#human-readable-expiration-times)
		- [Error Handling](#error-handling)
	- [📚 API Reference](#-api-reference)
		- [signJWT(secret, payload, expiration?)](#signjwtsecret-payload-expiration)
		- [verifyJWT(token, secret)](#verifyjwttoken-secret)
		- [parseHumanTimeToSeconds(timeExpression)](#parsehumantimetosecondstimeexpression)
	- [🧪 Testing](#-testing)
	- [⚖️ License](#-license)
	- [📧 Contact](#-contact)

## 📝 Description

> A robust JWT (JSON Web Token) utility library built on top of the Jose library.

**@nowarajs/jwt** provides a simple API for signing and verifying JSON Web Tokens with built-in error handling, human-readable time expressions, and comprehensive JWT claims management. Built with TypeScript and optimized for Bun runtime.

## 🔧 Installation

```bash
bun add @nowarajs/jwt @nowarajs/error
```

## ⚙️ Usage

### Basic JWT Operations

```ts
import { signJWT, verifyJWT } from '@nowarajs/jwt'

// Sign a JWT with default 15-minute expiration
const secret = 'your-secret-key'
const payload = { userId: '123', role: 'user' }

const token = await signJWT(secret, payload)

// Verify the JWT
const result = await verifyJWT(token, secret)
if (result)
  console.log('Valid token:', result.payload)
else
  console.log('Invalid or expired token')
```

### Advanced Usage with Custom Claims

```ts
import { signJWT } from '@nowarajs/jwt'

const token = await signJWT(
  'your-secret-key',
  {
    userId: '123',
    role: 'admin',
    permissions: ['read', 'write'],
    // Override default claims
    iss: 'MyApp',
    sub: 'user-123',
    aud: ['web-app', 'mobile-app']
  },
  '2 hours' // Human-readable expiration
)
```

### Human-Readable Expiration Times

The library supports various expiration formats:

```ts
// Numeric timestamp (seconds since epoch)
await signJWT(secret, payload, 1672531200)

// Date object
await signJWT(secret, payload, new Date('2024-12-31'))

// Human-readable strings
await signJWT(secret, payload, '15 minutes')
await signJWT(secret, payload, '2 hours')
await signJWT(secret, payload, '1 day')
await signJWT(secret, payload, '1 week')
await signJWT(secret, payload, '30 days')

// With modifiers
await signJWT(secret, payload, '+2 hours')
await signJWT(secret, payload, '1 hour from now')
```

### Error Handling

```ts
import { signJWT, verifyJWT } from '@nowarajs/jwt'
import { HttpError } from '@nowarajs/error'

try {
  const token = await signJWT('secret', { userId: '123' }, '-1 hour') // Past expiration
} catch (error) {
  if (error instanceof HttpError) {
    console.log('JWT Error:', error.message)
    console.log('Status Code:', error.httpStatusCode)
  }
}
```

## 📚 API Reference

You can find the complete API reference documentation for `jwt` at:

- [Reference Documentation](https://nowarajs.github.io/jwt/)

## ⚖️ License

Distributed under the MIT License. See [LICENSE](./LICENSE) for more information.

## 📧 Contact

- Github: [NowaraJS Organization](https://github.com/NowaraJS)
- Repository: [jwt](https://github.com/NowaraJS/jwt)

