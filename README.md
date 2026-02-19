# Wazuh Offline Installation – Single-Node

This repository provides a **simple offline deployment method** for installing the Wazuh server on **Ubuntu Server** using the **assisted single-node installation**.

The installer script is stored in the repository root, while the required offline packages are available in the **Releases section**.

---

# Repository Structure

Repository root:

wazuh-install.sh – Official Wazuh assisted installation script

GitHub Release assets:

wazuh-install-files.tar – Wazuh configuration and installation support files  
wazuh-offline.tar.gz – Wazuh packages required for offline installation  
offline-deps.zip – Ubuntu dependency bundle required by the installer  

These files must be downloaded from the **Release** and copied to the offline VM.

---

# 1. Prerequisites

Before starting:

- Ubuntu Server **24.04.3 recommended**
- amd64 / x86_64 system
- User with **sudo privileges**
- Hyper-V, VMware, VirtualBox, or physical system
- Internet access **only for downloading the release files**
- Offline VM with files transferred via:
  - shared folder
  - ISO
  - USB
  - SCP from staging host

---

# 2. Download Required Files

From the repository:

Download:
wazuh-install.sh

From the **Release section** download:

wazuh-install-files.tar  
wazuh-offline.tar.gz  
offline-deps.zip  

Copy all files to the VM, for example:

/opt/wazuh-offline/

Example:

mkdir /opt/wazuh-offline
cd /opt/wazuh-offline

---

# 3. Extract Files

Extract the dependency bundle:

unzip offline-deps.zip

Extract Wazuh offline packages:

tar -xvf wazuh-offline.tar.gz

Extract installation support files:

tar -xvf wazuh-install-files.tar

---

# 4. Install Required Offline Dependencies

Install all dependency packages first:

cd offline-deps
sudo dpkg -i *.deb

If dpkg reports dependency issues:

sudo dpkg -i *.deb || true
sudo apt install -f

This installs required packages such as:

debconf  
adduser  
procps  
apt-transport-https  
gnupg  
debhelper  
libcap2-bin  

and their dependencies.

---

# 5. Run the Wazuh Offline Installer

Return to the installation directory:

cd /opt/wazuh-offline

Make the script executable:

chmod +x wazuh-install.sh

Run the assisted offline installation:

bash wazuh-install.sh --offline-installation -a

The installer will automatically:

- Install Wazuh Manager
- Install Wazuh Indexer
- Install Wazuh Dashboard
- Generate TLS certificates
- Configure internal users
- Configure a single-node deployment

---

# 6. Verify Installation

Check service status:

sudo systemctl status wazuh-manager
sudo systemctl status wazuh-indexer
sudo systemctl status wazuh-dashboard

Check logs if necessary:

sudo tail -f /var/ossec/logs/ossec.log
sudo tail -f /var/log/wazuh-indexer/wazuh-cluster.log

---

# 7. Access the Wazuh Dashboard

Get the system IP address:

ip a

Open in browser:

https://<VM-IP>:5601

Default credentials:

User: admin

Reset password offline if needed:

sudo /usr/share/wazuh-dashboard/bin/wazuh-dashboard-users user passwd admin

---

# Notes

This installation method is designed for:

- Air-gapped environments
- Security labs
- SOC deployments
- Offline infrastructure
