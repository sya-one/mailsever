# mailsever.md
This guide walks you through installing a secure self-hosted mail server using **Mailcow** on a Debian/Ubuntu-based system.


# Install Fail2Ban
Fail2Ban protects your server from brute-force attacks.
`sudo apt install fail2ban -y`

# Install and Configure UFW Firewall
sudo apt install ufw -y
`sudo install ufw`

## # Default rules
`sudu ufw default deny incoming`
`sudo ufw default allow outgoing`
# Allow essential services
`sudo ufw allow 22,25,80,110,143,443,465,587,993,995,4190/tcp`
# Enable UFW
`sudo ufw enable`

# Mailserver

## Install Required Packages
`sudo apt install curl git -y`

## Install Docker
`su`

`curl -sSL https://get.docker.com/ | CHANNEL=stable sh`

`systemctl enable --now docker`

## Install Docker Compose
`sudo curl -sSL \
  https://github.com/docker/compose/releases/download/$(curl -Ls https://www.servercow.de/docker-compose/latest)/docker-compose-$(uname -s)-$(uname -m) \
  -o /usr/local/bin/docker-compose`

`chmod +x /usr/local/bin/docker-compose`

## Install the mail server
`su`

`cd /opt`

`git clone https://github.com/mailcow/mailcow-dockerized`

`cd mailcow-dockerized`

`./generate_config.sh`

# Start the mail server
`docker-compose pull`

`docker-compose up -d`
