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

## 3) Create an A record
- Type: `@`
- Name: `tanvirahmed.uk`
- Enter the public IPv4 address of your EC2 instance
- Turn `proxy status` to **DNS only**

## 4) Confirm DNS Propagation
   ```bash
   nslookup tanvirahmed.uk
   ```

Expected Output:

   ```bash
   Non-authoritative answer:
   Name:	tanvirahmed.uk
   Address: 54.90.111.24
   ```

---

## 5) SSH into the EC2 Instance

1. Move your key into a known location (example uses Downloads)
2. SSH into your EC2 instance:
   ```bash
   chmod 400 your-key.pem
   ssh -i ec2-key.pem ubuntu@<EC2_PUBLIC_IP>
   
## 6) Install and run NGINX 
In Terminal:

   ```bash
   sudo apt update
   sudo apt install -y nginx
   ```

## 7) Test if NGINX is running

1. In Terminal:
   ```bash
   curl localhost
   ```
2. Open: `http://<EC2_PUBLIC_IP>`or `tanvirahmed.uk`

You should see the default NGINX loading page

## 8)Configure NGINX for tanvirahmed.uk
      ```bash
      sudo vim /etc/nginx/conf.d/tanvirahmed.uk.conf
      ```

And paste:
```bash
server {
    listen 80;
    server_name yassinnginx.uk www.yassinnginx.uk;

    root /usr/share/nginx/html;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```
Then:
```bash
sudo nginx -t
sudo systemctl reload nginx
```

## 9)Customise the NGINX landing page
```bash
sudo nano /var/www/html/index.html
```
Paste:
```bash
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>tanvirahmed.uk | DevOps Project</title>
  <style>
    body { font-family: system-ui, -apple-system, Segoe UI, Roboto, Arial, sans-serif; margin: 0; padding: 0; }
    .wrap { max-width: 900px; margin: 0 auto; padding: 48px 20px; }
    .card { border: 1px solid #e5e7eb; border-radius: 14px; padding: 28px; }
    h1 { margin: 0 0 10px; font-size: 34px; }
    p { line-height: 1.6; color: #374151; }
    code { background: #f3f4f6; padding: 2px 6px; border-radius: 6px; }
    .meta { margin-top: 18px; font-size: 14px; color: #6b7280; }
    .badge { display: inline-block; padding: 6px 10px; border-radius: 999px; background: #eef2ff; color: #3730a3; font-weight:>
  </style>
</head>
<body>
<header style="
  background: #0f172a;
  color: #f8fafc;
  padding: 20px 0;
">
  <div style="
    max-width: 900px;
    margin: 0 auto;
    padding: 0 20px;
    display: flex;
    flex-direction: column;
    gap: 6px;
  ">
    <h1 style="margin: 0; font-size: 26px;">Tanvir Ahmed</h1>
    <p style="margin: 0; font-size: 15px; color: #c7d2fe;">
      DevOps Engineer
    </p>

    <div style="margin-top: 8px; font-size: 14px;">
      <a href="https://github.com/tanvir039" target="_blank"   <!-- Change the GitHub URL -->
         style="color: #93c5fd; text-decoration: none; margin-right: 14px;">
        GitHub
      </a>
      <a href="https://www.linkedin.com/in/tanvir-ahmed7/" target="_blank"  <!-- Change the LinkedIn URL -->
         style="color: #93c5fd; text-decoration: none;">
        LinkedIn
      </a>
    </div>
  </div>
</header>

  <div class="wrap">
    <div class="card">
      <span class="badge">Deployed on AWS EC2</span>
      <h1>NGINX-EC2 Project </h1>
      <p>This page is served by <code>NGINX</code> running on an <code>EC2</code> instance and reached via my domain.</p>

      <h2>What I practised</h2>
      <ul>
        <li>Launching an EC2 instance</li>
        <li>Configuring security groups (ports 22 &amp; 80)</li>
        <li>Installing and managing NGINX</li>
        <li>Pointing DNS (A record) to the EC2 public IP</li>
      </ul>

      <p class="meta">Last updated: <span id="ts"></span></p>
    </div>
  </div>
```
Then:
```bash
sudo systemctl reload nginx
```
