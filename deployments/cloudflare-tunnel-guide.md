
## Cloudflare Tunnel Setup

## Why Tunnels Instead of Port Forwarding

· ✅ No open ports on VPS (except SSH)
· ✅ Origin IP hidden
· ✅ Free DDoS protection
· ✅ Automatic SSL

## Setup Steps

1. Install cloudflared

```bash
curl -fsSL https://pkg.cloudflare.com/cloudflare-public-v2.gpg | sudo tee /usr/share/keyrings/cloudflare-public-v2.gpg >/dev/null
echo 'deb [signed-by=/usr/share/keyrings/cloudflare-public-v2.gpg] https://pkg.cloudflare.com/cloudflared any main' | sudo tee /etc/apt/sources.list.d/cloudflared.list
sudo apt update && sudo apt install cloudflared
```

2. Create Tunnel

```bash
cloudflared tunnel login
cloudflared tunnel create vigil
```

3. Configure

```yaml
# ~/.cloudflared/config.yml
tunnel: <tunnel-id>
ingress:
  - hostname: api.vigil247.app
    service: http://localhost:8000
  - service: http_status:404
```

4. Route DNS

```bash
cloudflared tunnel route dns vigil api.vigil247.app
sudo cloudflared service install
```

