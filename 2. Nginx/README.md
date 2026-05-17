# Practical 2: Deploy Nginx and expose it publicly

## Goal

Install Nginx on an Ubuntu cloud instance and make it available via public IP.

## Prerequisites

- AWS EC2 Ubuntu instance with a public IP
- Security group inbound rules:
  - SSH (22) from your IP
  - HTTP (80) from 0.0.0.0/0 (or your IP for limited access)
- MobaXterm on Windows

## Step 1: Connect to instance

Use MobaXterm SSH or:

```
ssh -i /path/to/key.pem ubuntu@PUBLIC_IP
```

## Step 2: Install and start Nginx

```
sudo apt update
sudo apt install -y nginx
sudo systemctl enable nginx
sudo systemctl start nginx
sudo systemctl status nginx
```

## Step 3: Verify from browser

Open:

```
http://PUBLIC_IP
```

You should see the default Nginx welcome page.

## Cleanup

- Stop or terminate the instance when done.
