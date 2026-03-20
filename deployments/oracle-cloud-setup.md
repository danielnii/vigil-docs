
# Oracle Cloud VPS Setup Guide

## Step-by-Step Setup

### 1. Create Instance

- Region: South Africa Central (Johannesburg)
- Image: Canonical Ubuntu 24.04
- Shape: VM.Standard.E2.1.Micro (Always Free)

### 2. Initial Server Setup

```bash
ssh -i your-key.pem ubuntu@your-ip
sudo apt update && sudo apt upgrade -y
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
sudo apt install docker-compose-plugin -y
```

### 3. Firewall Configuration

Oracle Cloud Console: Add ingress rule for port 8000 (source: 0.0.0.0/0)

### 4. DNS Configuration

```bash
sudo nano /etc/netplan/50-cloud-init.yaml
# Add nameservers: [8.8.8.8, 1.1.1.1]
sudo netplan apply
```

### 5. Deploy Application

```bash
git clone https://github.com/yourusername/vigil-backend.git
cd vigil-backend
docker compose up -d
```

