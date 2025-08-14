# Mail Server Installation Guide (Mailcow + Docker + SSL)

This guide explains how to install and configure a secure self-hosted mail server using **Mailcow** on a Debian/Ubuntu-based system, including firewall rules, anti-brute-force protection, DNS configuration, and automatic SSL with Let's Encrypt.

---

## 1. Install Fail2Ban

Fail2Ban protects your server from brute-force attacks.

```bash
sudo apt install fail2ban -y
```
## 2. Install and Configure UFW Firewall
```bash
sudo apt install ufw -y
```

# Default rules

```bash
sudo ufw default deny incoming
```
```bash
sudo ufw default allow outgoing
```

# Allow essential services
```bash
sudo ufw allow 22,25,80,110,143,443,465,587,993,995,4190/tcp
```

# Enable UFW
```bash
sudo ufw enable
```

## 3. Install Required Packages
```bash
sudo apt install curl git -y
```

## 4. Install Docker
```bash
sudo su
curl -sSL https://get.docker.com/ | CHANNEL=stable sh
```
```bash
systemctl enable --now docker
```

## 5. Install Docker Compose
```bash
sudo curl -sSL \
  https://github.com/docker/compose/releases/download/$(curl -Ls https://www.servercow.de/docker-compose/latest)/docker-compose-$(uname -s)-$(uname -m) \
  -o /usr/local/bin/docker-compose
```

```bash
sudo chmod +x /usr/local/bin/docker-compose
```

## 6. Install Mailcow
```
sudo su
```
```
cd /opt
```
```
git clone https://github.com/mailcow/mailcow-dockerized
```
```
cd mailcow-dockerized
```
```
./generate_config.sh
```

Note:
The generate_config.sh script will ask for your mail server hostname (e.g., mail.yourdomain.com).
Make sure the A record for this hostname points to your server IP before continuing.

## 7. Enable Let's Encrypt SSL
Open the generated `mailcow.conf` file and set:

``SKIP_LETS_ENCRYPT=n``

Then set:

``LETS_ENCRYPT_MAIL=postmaster@yourdomain.com``

## 8. Start the Mail Server
```
docker-compose pull
```
```
docker-compose up -d
```
## 9. Access the Mail Server
Web UI: `https://mail.yourdomain.com`

Default Admin Login: Found in `mailcow.conf` after installation.

## 10. Configure SPF, DKIM, and DMARC
SPF Record

``v=spf1 mx a ip4:YOUR_SERVER_IP -all``
DKIM Record

After Mailcow generates DKIM keys (in the admin panel):

Go to Mailcow UI → Configuration → ARC/DKIM keys

Copy the TXT record and add it to your DNS.

``default._domainkey.yourdomain.com  IN  TXT  "v=DKIM1; k=rsa; p=YOUR_PUBLIC_KEY"``

DMARC Record

``_dmarc.yourdomain.com  IN  TXT  "v=DMARC1; p=quarantine; rua=mailto:postmaster@yourdomain.com; ruf=mailto:postmaster@yourdomain.com; fo=1"``

## 11. Update and Maintain
To update Mailcow:
``cd /opt/mailcow-dockerized``

```
docker-compose pull
```
```
docker-compose up -d
```

## 12. Security Recommendations
```
sudo fail2ban-client status
```
