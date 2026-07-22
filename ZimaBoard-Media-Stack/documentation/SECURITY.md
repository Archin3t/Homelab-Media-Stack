# Security Policy — ZimaBoard Media Stack

This repository is **public documentation**.

Its purpose is to document media/books architecture and educational Homelab practices.

It must never contain secrets, private infrastructure details, or information that could assist unauthorized access.

---

# What Belongs in This Repository

This repository may contain:

- Architecture documentation
- System diagrams
- Role separation concepts
- Generic configuration examples
- Placeholder values
- Educational Homelab guidance
- Public-safe deployment patterns

Approved examples:

```text
<JELLYFIN_HOST>
<MEDIA_STACK_HOST>
<BOOKS_HOST>
<MEDIA_STORAGE>
```

Example paths and ports are acceptable when they do not expose private infrastructure.

Examples:

```text
/mnt/media
/books
:8096
:5055
:5050
```

---

# What Must Stay Private

Never commit:

## Credentials

- Passwords
- Default credentials
- API keys
- Access tokens
- Authentication secrets
- Indexer credentials

## Infrastructure Details

- Real LAN IP addresses
- Public DNS records
- Private hostnames
- Usernames
- Hardware inventory tied to identity

## Security Files

```text
.env
.pem
.key
credentials.json
secrets.yaml
```

## Screenshots

Before uploading screenshots, verify they do not reveal logins, tokens, private URLs, or internal IPs.

---

# Before Every Commit

- [ ] No real IP addresses or private DNS entries
- [ ] No passwords or default credentials
- [ ] No API keys or tokens
- [ ] No `.env` files staged
- [ ] No private certificates or key files
- [ ] No screenshots containing sensitive information

---

# If a Secret Is Accidentally Published

1. Rotate or revoke the exposed credential immediately
2. Remove the secret from the repository
3. Scrub Git history if it was pushed
4. Review related services for unauthorized access

---

# Credits & Attribution

Created and maintained by **Archin3t**

This security policy is part of the **ZimaBoard Media Stack** documentation project.

---

© 2026 Archin3t. All Rights Reserved.
