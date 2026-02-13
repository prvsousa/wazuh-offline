# Wazuh Offline Installation – Single-Node

This repository contains all files required to install the **Wazuh server** offline 
on **Ubuntu Server** in Hyper-V, using the **assisted single-node method**.

---

## Repository Contents

- `README.md` – This installation guide  
- `wazuh-install.sh` – Official Wazuh offline assisted install script  
- `wazuh-install-files.tar` – Additional Wazuh configuration files and scripts  
- `wazuh-offline.tar.gz` – Essential `.deb` packages and dependencies for offline installation  
- `wazuh-transfer.iso` – ISO containing all files above (optional)  
- `/deb/` – Optional folder with individual `.deb` packages if not using ISO  

> Included packages: `debconf`, `adduser`, `procps`, `apt-transport-https`, 
`gnupg`, `debhelper (≥9)`, `libcap2-bin` and all dependencies.

---

## 1. Prerequisites

- Ubuntu Server VM compatible with included packages  
- User with **sudo/root** access  
- Hyper-V or other hypervisor with access to ISO or shared folder  
- All `.tar.gz`, scripts and `.deb` files copied to the VM

---

## 2. Using the Offline ISO (Recommended)

1. Download `wazuh-transfer.iso` from **GitHub Release**  
2. Mount the ISO in the VM:

```bash
sudo mkdir -p /mnt/cdrom
sudo mount /dev/cdrom /mnt/cdrom
ls /mnt/cdrom
```

3. Copy files to a local folder:

```bash
mkdir -p ~/wazuh-offline
cp /mnt/cdrom/* ~/wazuh-offline/
cd ~/wazuh-offline
```

## 3 Install Essential Offline Packages

Install all `.deb` files using 
```bash 
cd offline-packages/
sudo dpkg -i *.deb
```

Fix dependencies if needed using `sudo apt install -f`.

> Ensures all packages required by Wazuh are installed:  
> `debconf`, `adduser`, `procps`, `apt-transport-https`, `gnupg`, `debhelper`, `libcap2-bin`.

---

## 4. Install Wazuh Offline

Make the script executable with `chmod +x wazuh-install.sh`.

Run assisted offline installation with:
```bash
bash wazuh-install.sh --offline-installation -a
```

- The script detects installed packages and configures a **single-node offline Wazuh**  
- Generates TLS certificates and internal users automatically

---

## 5. Verify Services

Check status of services:

- `sudo systemctl status wazuh-manager`  
- `sudo systemctl status wazuh-indexer`  
- `sudo systemctl status wazuh-dashboard`

Useful logs:

- `sudo tail -f /var/ossec/logs/ossec.log`  
- `sudo tail -f /var/log/wazuh-indexer/wazuh-cluster.log`

---

## 6. Access the Dashboard

Find the VM IP using `ip a`.

Open browser from host or internal network:

`https://<VM-IP>:5601`

- Default user: `admin`  
- Reset password offline:

`sudo /usr/share/wazuh-dashboard/bin/wazuh-dashboard-users user passwd admin`
