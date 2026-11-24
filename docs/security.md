# Security

## Remote access

- Remote access only through a Cloudflare tunnel with Cloudflare Access in front.
- No inbound ports are open to the internet.

## Encryption

- Trusted TLS certificates on internal names, from a Let's Encrypt wildcard certificate issued by DNS challenge.
- Encrypted DNS: AdGuard Home serves DNS over HTTPS and DNS over TLS.
- The remote DNS-over-HTTPS endpoint answers only on a private path. Requests to any other path are rejected.

## Administration

- Proxmox admin uses a named account with two-factor authentication and passkeys.
- SSH is key-only.
- The root account is kept for emergencies only.
- The management interface uses a trusted certificate.

## Secrets

- Cloudflare API tokens are limited to one zone, with minimal permissions and an expiry date.
- No secrets, addresses, or private domain names are stored in this repo.

## Known gaps I am closing

- Backups of host configuration and service data, with tested restores.
- Single sign-on across services.
