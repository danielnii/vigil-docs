
# iptables Hardening Guide

## The Safe Baseline

```bash
#!/bin/bash
# Save current rules
iptables-save > /root/iptables-backup-$(date +%Y%m%d).txt

# Set policies
iptables -P INPUT DROP
iptables -P FORWARD DROP
iptables -P OUTPUT ACCEPT

# Allow loopback (CRITICAL)
iptables -A INPUT -i lo -j ACCEPT
iptables -A OUTPUT -o lo -j ACCEPT

# Allow established connections
iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT

# Allow SSH
iptables -A INPUT -p tcp --dport 22 -j ACCEPT

# Save
netfilter-persistent save
```

## Docker-Specific Rules

Docker creates its own chains. Never delete DOCKER, DOCKER-USER, or DOCKER-FORWARD.

## The "I Broke Everything" Recovery

```bash
iptables -P INPUT ACCEPT
iptables -P FORWARD ACCEPT
iptables -P OUTPUT ACCEPT
iptables -F
iptables -X
iptables -t nat -F
iptables -t mangle -F
systemctl restart docker
```

