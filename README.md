# mailsever.md
This guide walks you through installing a secure self-hosted mail server using **Mailcow** on a Debian/Ubuntu-based system.

## 1. Install Fail2Ban

Fail2Ban protects your server from brute-force attacks.

```bash
sudo apt install fail2ban -y
