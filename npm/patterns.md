# Nginx Proxy Manager Patterns

## General Rules

- All public services use subdomains on `cedshomelab.com`.
- Cloudflare provides public SSL; NPM usually uses "None" cert internally.
- Backends with self-signed SSL (Proxmox, TrueNAS) use `proxy_ssl_verify off;`.

## Common Configs

### Proxmox

- Domain: `pve.cedshomelab.com`
- Scheme: `https`
- Forward IP: `10.10.30.250`
- Forward Port: `8006`

**Advanced:**

```nginx
proxy_ssl_verify off;
proxy_ssl_verify_depth 1;
proxy_ssl_name 10.10.30.250;
```

### TrueNAS

- Domain: `truenas.cedshomelab.com`
- Scheme: `https`
- Forward IP: `10.10.30.143`
- Forward Port: `443`

**Advanced:**

```nginx
proxy_ssl_verify off;
proxy_ssl_verify_depth 1;
proxy_ssl_name 10.10.30.143;
```

### Home Assistant

- Domain: `ha.cedshomelab.com`
- Scheme: `http`
- Forward IP: `10.10.30.104`
- Forward Port: `8123`
