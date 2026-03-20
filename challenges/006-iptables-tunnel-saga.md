# Challenge: The Great iptables Cleanup That Broke Everything

## Date
2026-03-16 — The Night of 1000 Debugs

## Timeline of Disaster

### 19:00 - Innocent Beginning
We decided to clean up old iptables rules for port 8000.

### 19:05 - The Mistake
```bash
sudo iptables-save | grep -v "dport 8000" | sudo iptables-restore
```

This removed rules for established connections and DNS!

19:10 - First Symptom

```
git pull: Could not resolve hostname github.com
```

DNS was broken. We fixed it with:

```bash
echo "nameserver 8.8.8.8" | sudo tee /etc/resolv.conf
```

20:00 - The Tunnel Died

```
cloudflared ERR Unable to reach origin service
```

20:30 - Discovery

The loopback interface lo rules were deleted! Cloudflared couldn't talk to localhost:8000.

21:00 - False Hope

Re-added loopback rules, but still broken. Docker networks were also affected:

```
Error response from daemon: Failed to Setup IP tables: Unable to enable ACCEPT OUTGOING rule
```

22:00 - The Fix

```bash
# Complete iptables reset
sudo iptables -P INPUT ACCEPT
sudo iptables -P OUTPUT ACCEPT
sudo iptables -P FORWARD ACCEPT
sudo iptables -F
sudo iptables -X
sudo iptables -t nat -F
sudo iptables -t mangle -F

# Minimal safe rules
sudo iptables -A INPUT -i lo -j ACCEPT
sudo iptables -A OUTPUT -o lo -j ACCEPT
sudo iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT

# Restart Docker (recreates its chains)
sudo systemctl restart docker
```

23:00 - Victory

```bash
curl https://api.vigil247.app/health
# {"status":"Vigil is watching"...}
```

Lessons Learned

1. NEVER flush iptables on a production server without a backup
2. Always save rules first: sudo iptables-save > ~/iptables-backup-$(date +%Y%m%d).txt
3. Docker needs its chains! (DOCKER, DOCKER-USER, DOCKER-FORWARD)
4. Loopback interface must be ACCEPT for localhost communication
5. DNS (port 53) is critical for outbound connections

The Safe iptables Template

```bash
#!/bin/bash
# Save current
iptables-save > /root/iptables-backup.txt

# Set policies
iptables -P INPUT DROP
iptables -P FORWARD DROP
iptables -P OUTPUT ACCEPT

# Allow loopback
iptables -A INPUT -i lo -j ACCEPT
iptables -A OUTPUT -o lo -j ACCEPT

# Allow established connections
iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT

# Allow SSH (change port if needed)
iptables -A INPUT -p tcp --dport 22 -j ACCEPT

# Allow HTTP/HTTPS for Docker/updates
iptables -A INPUT -p tcp --dport 80 -j ACCEPT
iptables -A INPUT -p tcp --dport 443 -j ACCEPT

# Allow DNS
iptables -A OUTPUT -p udp --dport 53 -j ACCEPT
iptables -A OUTPUT -p tcp --dport 53 -j ACCEPT

# Save
netfilter-persistent save