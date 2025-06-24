# 🎹+🎸=🔐 Keytar

A lightweight SSO mock service for development environments that simulates OAuth 2.0/OpenID Connect flows without actual security verification.

## Quick Start

### Build the Docker Image

```bash
npm run docker:build
# or
docker build -t keytar:latest .
```

### Drop-in KeyCloak Replacement

After building, simply change your Docker image in `docker-compose.yml`:

```yaml
keycloak:
  image: keytar:latest  # was: quay.io/keycloak/keycloak:latest
  environment:
    KC_HTTP_PORT: 8020
  ports:
    - "8020:8020"
  volumes:
    - ./data/keycloak/realm-config.json:/opt/keycloak/data/import/realm-config.json
  command: ["start-dev", "--import-realm"]  # Keytar ignores arguments
```

Keytar automatically:
- Accepts `KC_HTTP_PORT` as an alias for `PORT`
- Looks for realm config at `/opt/keycloak/data/import/realm-config.json`
- Provides a `start-dev` script that ignores all arguments
- Routes KeyCloak-style URLs to the correct endpoints

### Local Development

```bash
npm install
PORT=8020 REALM_CONFIG=./realm-config.json npm start
```

## Environment Variables

- `PORT` - Server port (default: 8020)
- `REALM_CONFIG` - Path to realm configuration JSON (default: /config/realm-config.json)
- `TOKEN_EXPIRY` - Override token expiration in seconds (default: 86400)
- `DEBUG` - Enable debug logging (default: false)

## Endpoints

- **GET /auth** - OAuth 2.0 authorization endpoint
- **GET /get-token?username=xxx** - Programmatic token generation
- **GET /userinfo** - Get user info from bearer token
- **GET /health** - Health check

## Features

- 🚀 Instant authentication - no passwords required
- 📋 Reads existing KeyCloak realm configuration
- 🎯 User selection interface with bouncing music note loading animation
- 🔧 Programmatic token generation for testing
- 📦 Minimal Docker image (~100MB)
- ⚡ Stateless design

## Security Warning

⚠️ **Development Use Only** - This service intentionally bypasses all security checks for ease of development.