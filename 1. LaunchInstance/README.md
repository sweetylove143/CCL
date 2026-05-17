# Practical 1: Launch EC2 Ubuntu and connect with MobaXterm

## Goal

Launch an Ubuntu EC2 instance and connect to it from Windows using MobaXterm and SSH.

## Prerequisites

- AWS account
- MobaXterm installed on Windows
- Internet access

## Step 1: Launch EC2 instance (AWS Console)

1. Open AWS Console and go to EC2.
2. Click Launch instance.
3. Name: Ubuntu-Server.
4. AMI: Ubuntu Server 22.04 LTS.
5. Instance type: t2.micro (free tier).
6. Key pair: Create new key pair
   - Name: my-key
   - Type: RSA
   - Format: .pem
   - Download and store the .pem file safely.
7. Network settings:
   - Allow SSH (port 22) from your IP (recommended) or 0.0.0.0/0 (for demo only).
8. Click Launch instance.

## Step 2: Get the public IP

- Wait until the instance state is Running.
- Copy the Public IPv4 address.

## Step 3: Connect using MobaXterm

1. Open MobaXterm -> Session -> SSH.
2. Remote host: Public IP.
3. Specify username: ubuntu.
4. Advanced SSH settings -> Use private key -> select my-key.pem.
5. Click OK and accept the fingerprint prompt.

## Step 4: Verify connectivity

You should see a shell prompt like:

```
ubuntu@ip-...:~$
```

## Optional: Connect from Git Bash / PowerShell

```
chmod 400 /path/to/my-key.pem
ssh -i /path/to/my-key.pem ubuntu@PUBLIC_IP
```

## Cleanup

- Stop or terminate the instance when done to avoid charges.
