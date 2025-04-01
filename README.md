# dms-certbot-nextcloud
Docker Mail Server with Certbot, Hetzner DNS and Nextcloud

This repository includes container services: docker-mailserver, Nextcloud, and Certbot with Hetzner DNS automation for SSL certificates.

`I chose Debian 12 arm64 for this, but it should work on most Linux x86_64 distros`

### Features

- Mail Server (docker-mailserver)
- Supports IMAP, SMTP, and secure email protocols
- SpamAssassin, ClamAV and Fail2Ban
- Uses Let's Encrypt SSL certificates
- Nextcloud for file storage and collaboration
- Certbot + Hetzner DNS API for automated SSL certificate generation

# Installation & Setup

## 1. Clone the Repository

```bash
git clone https://github.com/ea-de/dms-certbot-nextcloud.git
```

## 2. Get Hetzner API Token

Go to Hetzner DNS Console, create an API Token and save it securely.

## 3. Configure Certbot for Hetzner

Create a credentials file: ./certbot/config/hetzner.ini:

```bash 
dns_hetzner_api_token = YOUR_HETZNER_API_TOKEN 
```

Set the correct permissions:

```bash
chmod 600 ./certbot/config/hetzner.ini 
```

## 4. Get Certbot ready first
```bash
docker compose up certbot
```

## 5. If successful, proceed to launch the mail server:

```bash
docker compose up -d mailserver
```

- Setup an Email Account

```bash 
docker exec -it mailserver setup email add me@my-new-email.com 
```

## 6. Automate Certificate Renewal

Add a cron job to auto-renew SSL certificates:

```bash
0 3 * * * docker compose run certbot renew && docker compose restart mailserver 
```

## 7. DNS & Firewall  

- Ensure your DNS records (MX, SPF, DKIM, DMARC) are set up for your domain
- Nextcloud only needs an A record
- Verify firewall ports to allow email traffic and https for Nextcloud (25, 465, 587, 993, 443)

## 8. Access Nextcloud

- In your browser, enter your-domain:8080

## 9. Next up

- Configure Nextcloud behind a reverse proxy
- More details on DMARC & DKIM
