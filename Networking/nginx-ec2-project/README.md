# NGINX on AWS EC2 + Cloudflare DNS

This guide walks you through deploying an **NGINX web server** on an **Ubuntu EC2 instance** and making it reachable via your **custom domain** using **Cloudflare DNS**.


---
## 1) Buy a domain
1. Purchase your domain from Google Domains, GoDaddy, Cloudlfare, Namecheap
2. Go to domain settings and ensure you have the correct nameservers set:

   `jaime.ns.cloudflare.com`
   
    `peaches.ns.cloudflare.com`

   *These are Cloudflare's nameservers*

## 2) Launch an EC2 Instance (Ubuntu)

1. Log into the **AWS Console**
2. Go to **EC2 → Instances → Launch instance**
3. Set:
   - **Name**: `nginx-server`
   - **AMI**: `Ubuntu Server 22.04 LTS`
   - **Instance type**: `t3.micro` (Free Tier eligible on many accounts)
4. **Key pair (login)**:
   - Create a new key pair (RSA, `.pem`)
   - Download the file (e.g. `ec2-nginx-key.pem`)
5. **Network settings / Security Group inbound rules**:
   - Allow SSH:
     - Type: `SSH`
     - Port: `22`
     - Source: `My IP`
   - Allow HTTP:
     - Type: `HTTP`
     - Port: `80`
     - Source: `Anywhere (0.0.0.0/0)`
6. Click **Launch instance**
7. Wait until:
   - Instance state: `Running`
   - Status checks: `2/2 checks passed`

---

## 2) Copy the EC2 Public IP

1. Click your instance
2. Copy the **Public IPv4 address**
   - Example: `54.x.x.x`

You will use this for:
- SSH access
- Cloudflare DNS A record

3. Create an **A record**, named `@`, enter the public IPv4 address of your EC2 instance and turn `proxy status` to **DNS only**

---

## 3) SSH into the EC2 Instance

1. Move your key into a known location (example uses Downloads)
2. SSH into your EC2 instance:
   ```bash
   chmod 400 your-key.pem
   ssh -i ec2-key.pem ubuntu@<EC2_PUBLIC_IP>
   
## 4) Install and run NGINX 
In Terminal:

   ```bash
   sudo apt update
   sudo apt install -y nginx
   ```

## 5) Test if NGINX is running

1. In Terminal:
   ```bash
   curl localhost
   ```
2. Open: `http://<EC2_PUBLIC_IP>`

