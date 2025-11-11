# MarketPeak Ecommerce – Live on AWS EC2 (Amazon Linux)

**Live URL:**  
`http://$(curl -s http://169.254.169.254/latest/meta-data/public-ipv4)`  
*(Replace with your actual public IP after launch)*

**Status:** LIVE & WORKING  
**Location:** Nigeria  
**Time:** November 10, 2025 – 11:21 PM WAT

---

## Project Overview

This is a **fully deployed, responsive ecommerce website** using the **Wedding Lite Bootstrap 5 Template** (currently used as a placeholder for MarketPeak Ecommerce).  
Hosted on **AWS EC2** with **Apache HTTP Server (httpd)** on **Amazon Linux 2023**.

---

## Tech Stack

| Component        | Technology Used                     |
|------------------|-------------------------------------|
| Cloud Provider   | AWS EC2 (t3.micro or similar)       |
| OS               | Amazon Linux 2023                   |
| Web Server       | Apache HTTP Server (`httpd`)        |
| Frontend         | HTML5, CSS3, Bootstrap 5, JS        |
| Source Control   | Git + GitHub                        |
| Access           | SSH (port 22), HTTP (port 80)       |

---

## Deployment Steps (One-Time Setup)

```bash
# 1. Update system
sudo yum update -y

# 2. Install Apache & Git
sudo yum install httpd git -y

# 3. Start and enable Apache
sudo systemctl start httpd
sudo systemctl enable httpd

# 4. Clone your website from GitHub
sudo rm -rf /var/www/html/*
sudo git clone https://github.com/pakinsa/MarketPeak_Ecommerce.git /var/www/html/

# 5. Set correct permissions
sudo chown -R apache:apache /var/www/html/
sudo chmod -R 755 /var/www/html/
sudo restorecon -R -v /var/www/html/
```

---

## Verify It’s Working

```bash
# Local test (on EC2)
curl http://localhost | head -5

# Get public IP
curl -s http://169.254.169.254/latest/meta-data/public-ipv4
```

Open in browser:  
`http://<YOUR-PUBLIC-IP>`

You should see:  
> **Wedding Lite - Free Bootstrap 5 HTML Template**

*(Your MarketPeak design will replace this once updated in GitHub)*

---

## AWS Security Group (MUST BE OPEN)

| Type     | Protocol | Port | Source       |
|---------|----------|------|--------------|
| HTTP    | TCP      | 80   | `0.0.0.0/0`  |
| HTTPS   | TCP      | 443  | `0.0.0.0/0`  |
| SSH     | TCP      | 22   | `Your IP/32` |

> **Do NOT delete or change the SSH rule** — you’ll lose access!

---

## Maintenance Commands

```bash
# Restart Apache
sudo systemctl restart httpd

# Check Apache status
sudo systemctl status httpd

# Update site from GitHub
cd /var/www/html/
sudo git pull origin main
sudo chown -R apache:apache .
sudo restorecon -R -v .

# View logs
sudo journalctl -u httpd -n 50 --no-pager
```

---

## Security & Best Practices

- [ ] Use **Elastic IP** for static public IP
- [ ] Add **Let’s Encrypt SSL** (HTTPS) – *see below*
- [ ] Restrict SSH to your IP only
- [ ] Remove `.git` folder in production:
  ```bash
  sudo rm -rf /var/www/html/.git
  ```

---

## Add HTTPS (FREE SSL) – Run This Now

```bash
sudo yum install certbot python3-certbot-apache -y
sudo certbot --apache -d $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4)
```

> Follow prompts → choose **Redirect HTTP to HTTPS**

Your site will now be:  
`https://<YOUR-PUBLIC-IP>` (with green padlock)

---

## Project Files Location

```
/var/www/html/
├── index.html
├── css/
├── js/
├── images/
└── fonts/
```

---

## Team Access

| Role       | Access Method                     |
|-----------|-----------------------------------|
| Developer | `git pull` → update via GitHub    |
| Admin     | SSH: `ssh ec2-user@<PUBLIC-IP>`   |
| Visitors  | `http://<PUBLIC-IP>` or HTTPS     |

---

## Troubleshooting

| Issue                     | Fix |
|--------------------------|-----|
| Site not loading         | Check Security Group (port 80) |
| Wrong page showing       | Run `sudo git pull` in `/var/www/html/` |
| Permission denied        | Run `sudo chown -R apache:apache /var/www/html/` |
| SELinux errors           | Run `sudo restorecon -R -v /var/www/html/` |

---

![final_load_website](./images/final_load_website.png)