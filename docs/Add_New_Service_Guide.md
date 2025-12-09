# Adding a New Service to Cloudflare Tunnel + NGINX Proxy Manager

## Architecture Flow

```text
Browser
  │
  ▼
Cloudflare DNS (cedshomelab.com)
  │
  ▼
Cloudflare Edge (SSL, Zero Trust)
  │
  ▼
Cloudflare Tunnel (cloudflared)
  │
  ▼
NGINX Proxy Manager (10.10.30.210)
  │
  ▼
Internal Service (10.10.30.X:PORT)
```

---

## Step-By-Step Checklist

### 1. Gather Service Info

- **Subdomain:** `service.cedshomelab.com`
- **Internal IP:** `10.10.30.X`
- **Port:** `PORT`
- **Scheme:** `http` or `https`

Test local access:
- LAN: `http://10.10.30.X:PORT`

---

### 2. (Optional) Create DNS Route in Tunnel

Inside NPM container:

```bash
cloudflared tunnel route dns npm-tunnel service.cedshomelab.com
```

---

### 3. Add Proxy Host in NGINX Proxy Manager

Open NPM: `http://10.10.30.210:81`

Go to:
**Hosts → Proxy Hosts → Add Proxy Host**

**Details tab:**

- **Domain Names:** `service.cedshomelab.com`
- **Scheme:** `http` or `https`
- **Forward Hostname/IP:** `10.10.30.X`
- **Forward Port:** `PORT`

Enable:
- Block Common Exploits
- Websockets Support

---

### 4. HTTPS Self-Signed Backends (Optional)

If backend uses self-signed HTTPS (e.g., Proxmox, TrueNAS):

In NPM **Advanced tab:**

```nginx
proxy_ssl_verify off;
proxy_ssl_verify_depth 1;
proxy_ssl_name 10.10.30.X;
```

---

### 5. SSL Tab

Since Cloudflare provides SSL:

- **SSL Certificate:** `None`
- Force SSL: Off
- HSTS: Off
- HTTP/2: Off

Save.

---

### 6. Test External Access

Open:
`https://service.cedshomelab.com`

If issues:
- 404: hostname not in ingress
- 502/504: NPM can’t reach backend
- Cloudflare 52x: tunnel or DNS issue

---

## Notes

- Wildcard routing `*.cedshomelab.com` sends all new subdomains to NPM automatically.
- Only NPM needs to be updated for new services.
- No need to open router ports.

---

## Example

For Jellyfin on `http://10.10.30.143:30013`:

```bash
cloudflared tunnel route dns npm-tunnel jellyfin.cedshomelab.com
```

In NPM:
- Domain: `jellyfin.cedshomelab.com`
- Scheme: `http`
- IP: `10.10.30.143`
- Port: `30013`
- Block Exploits: On
- Websockets: On

Save & test:
`https://jellyfin.cedshomelab.com`
