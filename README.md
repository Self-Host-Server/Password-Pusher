# Password-Pusher

Self-hosted deployment config for [Password Pusher](https://github.com/pglombardo/PasswordPusher) via Docker Compose.

Password Pusher lets you share passwords, files, and other secrets through
a link that expires after a set number of views or days, instead of sending
them in plain text over email or chat.

## Quick start

1. Copy the example environment file and fill in your domain:

   ```bash
   cp .env.example .env
   ```

2. (Optional but recommended) Set `TLS_DOMAIN` in `.env` to a domain with DNS
   already pointed at this host. This enables automatic HTTPS/TLS via Let's
   Encrypt on port 443. Without it, the app is served over plain HTTP on
   port 80.

3. Start the stack:

   ```bash
   docker compose up -d
   ```

4. Open the app:
   - With `TLS_DOMAIN` set: `https://<your-domain>`
   - Without it: `http://localhost`

5. Stop the stack:

   ```bash
   docker compose down
   ```

## Configuration

All configuration is set via environment variables in [compose.yml](compose.yml)
(or in `.env`, loaded via `env_file`). See the inline comments in `compose.yml`
for the most common options (TLS, mail/SMTP, file storage backends, push
expiry defaults, branding, etc.), and the full reference in the
[Password Pusher self-hosted configuration docs](https://docs.pwpush.com/docs/self-hosted-configuration/).

This stack runs PostgreSQL as the database (`postgres` service), with data
persisted in the `pwpush-postgres-data` Docker volume. Credentials are set
via `POSTGRES_USER` / `POSTGRES_PASSWORD` / `POSTGRES_DB` in `.env` — change
the default password before deploying. File uploads persist in the
`pwpush-storage` Docker volume.

> **Note:** Password Pusher authenticates with email/password + optional
> TOTP two-factor auth (via Devise). It does not support OIDC/OAuth/SAML SSO.

## Development tooling

This repo uses [tox](https://tox.wiki/) to run linting and formatting checks:

```bash
tox            # lint, txt-lint, format-check, prettier, toml-lint
tox -e format  # auto-format YAML/JSON/Markdown/TOML in place
tox -e github  # full CI check chain
```

These checks (Ruff, textlint, Prettier, Taplo) run automatically on push and
pull request via [GitHub Actions](.github/workflows/tests.yml).
