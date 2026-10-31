# NETWORKWALKS-B084_-Week1-Cybesecurity_lab_Setup
<div align="center">

# 🔐 Cybersecurity Lab Environment Setup

**Building an isolated virtual lab for penetration testing and ethical hacking practice**
</div>

<p align="center">
  <img src="https://img.shields.io/badge/Skill-Cybersecurity-404040?style=flat-square&labelColor=C00000" />
  <img src="https://img.shields.io/badge/Ver-Virtualbox%20-0070C0?style=flat-square&labelColor=000000" />
  <img src="https://img.shields.io/badge/Kali%20Linux-E87500?style=flat-square&labelColor=000000&logo=kalilinux&logoColor=white" />
  <img src="https://img.shields.io/badge/Skill-Linux-404040?style=flat-square&labelColor=C00000" />
  <img src="https://img.shields.io/badge/Network-10.0.0.0%2F24-238F89?style=flat-square&labelColor=000000" />
  <img src="https://img.shields.io/badge/Penetration%20Testing-C00000?style=flat-square&labelColor=000000&logo=kalilinux&logoColor=white" />
  <img src="https://img.shields.io/badge/Skill-Virtualization-404040?style=flat-square&labelColor=C00000" />
  <img src="https://img.shields.io/badge/GitHub-404040?style=flat-square&labelColor=0070C0&logo=github&logoColor=white" />
  <img src="https://img.shields.io/badge/Kali%20Linux-404040?style=flat-square&labelColor=C00000&logo=kalilinux&logoColor=white" />
  <img src="https://img.shields.io/badge/NetworkWalks-404040?style=flat-square&labelColor=C00000" />
  <img src="https://img.shields.io/badge/Ethical%20Hacking-E87500?style=flat-square&labelColor=000000&logo=kalilinux&logoColor=white" />
  <img src="https://img.shields.io/badge/senamibonou-C00000?style=flat-square" />
</p>

---

## 📌 Project Overview

This project focuses on setting up a **virtual cybersecurity and penetration-testing laboratory** using VirtualBox and Kali Linux.

The purpose of the lab is to create a controlled environment where cybersecurity tools, network scanning, reconnaissance, vulnerability assessment, and other security-testing activities can be performed safely and repeatedly.

The lab is configured on a private virtual network so that additional machines can be added later and used as targets for authorized security testing.

---


## 🎯 Objectives

The main objectives of this project are to:

- Install and configure VirtualBox.
- Install/import Kali Linux as a virtual machine.
- Create a private **NAT Network** for the cybersecurity lab.
- Configure network connectivity for Kali Linux.
- Assign a consistent IP address to the Kali VM.
- Verify network connectivity and DNS resolution.
- Take a clean VM snapshot for recovery.
- Document the complete setup process.
- Prepare the environment for future cybersecurity projects.

  ## 🛡️ Purpose of the Lab

The lab provides an isolated and controlled environment for cybersecurity learning and authorized security testing.

It can be used for activities such as:

- Network reconnaissance
- Port scanning
- Vulnerability assessment
- Packet analysis
- Web security testing
- Exploitation practice
- Security-tool experimentation

⚠️ **Important:** This laboratory must only be used for systems that you own or have explicit permission to test. Do not use the lab or its tools to attack unauthorized systems.

## Tools Used

- **7-Zip**: archive extraction
- **VirtualBox**: virtualization software
- **Kali Linux**: security-oriented Linux distribution

## Step 1: Install 7-Zip

1. Download the installer from https://7-zip.org/download.html
2. Run the installation with the default options

7-Zip is used to extract the Kali Linux image, which is provided as a `.7z` archive.

## Step 2: Install VirtualBox

1. Download VirtualBox from https://virtualbox.org/wiki/Downloads
2. Run the installation with the default options

## Step 3: Create the NAT Network (NATNetwork)

1. In VirtualBox: **File → Tools → Network Manager**
2. Go to the **NAT Networks** tab and click **Create**
3. Name: `NATNetwork`
4. IPv4 Prefix: `10.0.0.0/24`
5. Enable DHCP (optional), then apply

This network isolates the virtual machines from the real network, while still allowing them to communicate with each other and access the Internet.

## Step 4: Import the Kali Linux Virtual Machine

1. Download the VirtualBox image of Kali from https://www.kali.org/get-kali/ (*Virtual Machines* section)
2. Extract the `.7z` archive with 7-Zip
3. In VirtualBox: **Machine → Add**, then select the extracted `.vbox` file

## Step 5: Configure the IP Address of Kali Linux

1. With the machine powered off: **Settings → Network → Adapter 1**
2. Attached to: **NAT Network**, Name: `NATNetwork`
3. Start the machine 
4. If the "Wired connection 1" connection is set to **Automatic (DHCP)**, switch it to **Manual** (IPv4 Method: Manual)
5. Restart the connection with `nmcli` to apply the changes:

```bash
sudo nmcli con show
sudo nmcli con down "Wired connection 1"
sudo nmcli con up "Wired connection 1"
```
6. Verify the configuration:

```bash
ip a
ip route
ping -c 3 10.0.0.1
ping -c 3 8.8.8.8
```

The first `ping` tests the gateway, the second tests Internet access.

## Step 6: Take a Snapshot

1. Select the Kali virtual machine in VirtualBox
2. Open the **Snapshots** tab
3. Click **Take**
4. Name the snapshot (e.g. `Kali - initial configuration`) and add a description (e.g. `IP 10.0.0.2, NATNetwork configured`)
5. Click **OK**

A snapshot saves the exact state of the VM, so I can restore it if a later change breaks the machine.

## Issue Encountered: The Installation Restarts at Every Boot

**Problem**: after the installation, the machine launched the Kali installer again at every startup.

**Cause**: the ISO image was still attached to the virtual optical drive, and the machine booted from it first.

**Solution**:
1. Power off the machine
2. **Settings → Storage**
3. Select the optical drive containing the `.iso` file
4. Click the disc icon on the right, then **Remove Disk from Virtual Drive**
5. Check in **System → Boot Order** that the **hard disk** is listed before the optical drive

**Key takeaway**: once the installation is finished, always remove the ISO image from the storage settings.

