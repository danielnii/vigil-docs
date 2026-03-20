
# SSH Best Practices

## Key Configuration

```bash
# Edit SSH config
sudo nano /etc/ssh/sshd_config

# Recommended settings
Port 2222  # Change from default 22
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
AllowUsers ubuntu
```

## Create SSH Key (if you don't have one)

```bash
ssh-keygen -t ed25519 -C "your-email@example.com"
```

## Copy Key to Server

```bash
ssh-copy-id -i ~/.ssh/id_ed25519.pub ubuntu@your-server
```

## Restart SSH

```bash
sudo systemctl restart sshd
```

