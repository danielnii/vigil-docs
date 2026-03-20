
# Cloudflare Security

## Key Features

- IP Hiding: Orange cloud hides your origin IP
- Free SSL: Automatic HTTPS certificates
- DDoS Protection: Unmetered mitigation
- WAF: Basic OWASP protection on free tier

## Critical Settings

- SSL/TLS: Set to "Full (strict)"
- Always Use HTTPS: Enable
- Minimum TLS Version: 1.2

## Firewall Rules (Recommended)

```bash
# Allow only Cloudflare IPs (if not using tunnel)
curl -s https://www.cloudflare.com/ips-v4 | while read ip; do
    iptables -A INPUT -p tcp -s $ip --dport 80 -j ACCEPT
    iptables -A INPUT -p tcp -s $ip --dport 443 -j ACCEPT
done
```

