# Aria Residences — deployment

## Requirements
- Node.js 18+ (nothing else — dependencies are bundled)

## Run
```bash
PORT=3000 HOSTNAME=0.0.0.0 node server.js
```
The app listens on the given port (default 3000) on all interfaces.

## Reverse proxy (recommended)
Put Caddy/Nginx in front for HTTPS + your domain. Caddy example:

```caddyfile
example.com {
    reverse_proxy 127.0.0.1:3000
}
```

Keep the app's own COOP/COEP response headers (they enable the fast
SharedArrayBuffer splat sorting). HTTPS is required for the phone
motion-sensor controls (deviceorientation).

## Caching
/models/* and /hdri/* are served with
`Cache-Control: public, max-age=31536000, immutable` — visitors download
them once. If you replace a model, ship it under a new filename.

## Process management (optional)
```bash
# run under a supervisor, e.g. pm2:
pm2 start server.js --name aria
```
