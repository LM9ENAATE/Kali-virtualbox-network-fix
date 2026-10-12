# Kali-virtualbox-network-fix
A quick step-by-step guide and bash script to fix "Network is unreachable" internet issues in Kali Linux on VirtualBox over Wi-Fi without Ethernet cables.
              Markdown
# 🌐 Fix Kali Linux No Internet Issue in VirtualBox (Wi-Fi / No Cable)

A quick and direct troubleshooting guide for fixing internet connectivity issues in Kali Linux running inside VirtualBox without an Ethernet cable.

---

## 📌 Problem Overview
* **Issue:** Kali Linux VM has no internet connection or Firefox fails to load pages.
* **Error:** `Network is unreachable` when pinging `8.8.8.8`.
* **Cause:** VirtualBox failed to obtain an IP via DHCP on `eth0` over Wi-Fi, or NetworkManager failed to bind properly.

---

## 🚀 The Solution (Step-by-Step)

### Option 1: One-Line Commands (Immediate Fix)

Open your Kali Linux **Terminal** and run the following commands in sequence:

```bash
# 1. Bring the network interface up
sudo ip link set eth0 down
sudo ip link set eth0 up

# 2. Assign static network parameters (VirtualBox NAT Range)
sudo ip addr add 10.0.2.15/24 dev eth0
sudo ip route add default via 10.0.2.2

# 3. Configure Google DNS
echo "nameserver 8.8.8.8" | sudo tee /etc/resolv.conf
Verify Connection:
Bash
ping -c 3 8.8.8.8
Option 2: Permanent Fix (Auto-Fix Script)
Since static route configurations reset upon reboot, you can automate this setup using a shell script.

1. Create the script:
Bash
nano fix-net.sh
2. Paste the following script:
Bash
#!/bin/bash
# Auto Network Fix for VirtualBox Kali Linux

sudo ip link set eth0 up
sudo ip addr add 10.0.2.15/24 dev eth0 2>/dev/null
sudo ip route add default via 10.0.2.2 2>/dev/null
echo "nameserver 8.8.8.8" | sudo tee /etc/resolv.conf > /dev/null

echo "[+] Network fixed successfully!"
ping -c 3 8.8.8.8
3. Save and set permissions:
Save in Nano: Ctrl + O -> Enter -> Ctrl + X

Make it executable:

Bash
chmod +x fix-net.sh
4. Run anytime after reboot:
Bash
./fix-net.sh
⚙️ VirtualBox Recommended Settings
Ensure your VM network settings are configured as follows:

Attached to: NAT

Adapter Type: Intel PRO/1000 MT Desktop (82540EM)

Cable Connected: Checked [X]
