# T-Pot-Honeypot-on-Azure-24.04.1-

![image](https://github.com/user-attachments/assets/f14884a4-f904-4394-8b8b-04052bee3e85)

![image](https://github.com/user-attachments/assets/4da08127-1b06-4a98-8927-3e297e872258)

![image](https://github.com/user-attachments/assets/a6a99d30-edcd-4f75-b32f-414333dc8e01)

# T-Pot Honeypot Setup on Azure

## 📃 Project Overview

Deploy a fully functional T-Pot honeypot in Microsoft Azure with security hardening, visualization, and active monitoring using Docker and T-Pot's modular architecture.

---

## 🚀 Deployment Summary

### 1. Create the Azure VM

* Provisioned an Ubuntu 20.04 LTS VM in Azure.
* Chose a VM size appropriate for Docker containers (e.g., Standard B2s).
* Created and used an SSH key pair for secure login.
* Disabled password-based authentication.

### 2. Configure Networking

* Attached a Network Security Group (NSG) with the following custom rules:

  * Allow SSH on port `64295` from a specific home IP address
  * Allow T-Pot dashboard access (port `8080`)
  * Block all other inbound traffic by default

![image](https://github.com/user-attachments/assets/eba2e492-f5d8-48c2-9654-c23fc5a1feb3)

### 3. SSH Access

* Connected to the VM using the following command:

  ```bash
  ssh -i "/path/to/private_key.pem" azureuser@[YOUR_PUBLIC_IP] -p 64295
  ```

### 4. Install T-Pot

* Cloned the T-Pot installation repo and ran the installer:

  ```bash
  git clone https://github.com/telekom-security/tpotce
  cd tpotce/iso/installer
  sudo ./install.sh
  ```
* Selected "INSTALL" and chose the **default configuration** (option "H" for Hardened).
* Rebooted VM after installation.

---

## 🔒 Hardening Steps

### Network Access

* Restricted SSH (port 64295) to only my trusted IP.
* Added NSG rule to only allow port `8080` for the web dashboard.

### Fail2Ban Installation

* Installed fail2ban to mitigate brute-force attempts:

  ```bash
  sudo apt update && sudo apt install fail2ban -y
  sudo systemctl enable fail2ban
  sudo systemctl start fail2ban
  ```

### SSH Key Rotation

* Generated a fresh SSH key:

  ```bash
  ssh-keygen -t rsa -b 4096 -f ~/Downloads/tpot_newkey.pem
  ```
* Added the new public key to `~/.ssh/authorized_keys` on the server.

---

## 📊 Visualization & Monitoring

### Accessing the Dashboard

* Navigated to: `http://[YOUR_PUBLIC_IP]:8080`
* Dashboard UI loaded with access to:

  * Attack Map
  * Cyberchef
  * Elasticvue
  * Kibana
  * Spiderfoot

### Live Attack Monitoring

* Observed live global attack attempts in real-time using the map.
* Events categorized by:

  * IP address
  * Country of origin
  * Honeypot type (e.g., Cowrie, Conpot, etc.)
  * Service targeted (e.g., TELNET, SSH)

---

## 🚫 Redactions & Privacy Notes

* All public IPs, private keys, and usernames were excluded or generalized.
* No `.pem` or sensitive configuration files were uploaded to this repo.
* Monitoring and logging are anonymized.

---

## 🚧 Next Steps

* Set up automatic backups or snapshots.
* Monitor and tune fail2ban configurations.
* Explore forwarding logs to a SIEM or log aggregator.

---

## 🎓 Learning Outcome

* Deployed a production-grade honeypot architecture on Azure.
* Implemented secure SSH practices, NSG restrictions, and live monitoring.
* Gained visibility into real-world attack telemetry.

