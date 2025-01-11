# How to install and configure Active Directory (AD) on Windows Server 2025 (Evaluation Version) using a Proxmox virtual machine 

This guide will walk you through the process of installing and configuring Active Directory (AD) on a Windows Server machine using the evaluation version and running it as a virtual machine in Proxmox. This setup allows you to test and experiment with Active Directory features without needing a paid license. It can be done on a physical or virtual machine.

**Prerequisites**

-Windows Server Evaluation Version (can be downloaded for free from Microsoft's site).

-A Proxmox host (local or remote) 

-Basic knowledge of Proxmox and Windows Server.

**Step 1: Download Windows Server Evaluation Version**

1. Go to the [Microsoft Evaluation Center.](https://www.microsoft.com/en-us/evalcenter/download-windows-server-2025)
2. Choose the Windows Server version you want (usually Windows Server 2022 or Windows Server 2019).
3. Download the ISO file for the evaluation version (it is typically valid for 180 days).
4. You will need to sign up or log in with a Microsoft account to download.

**Step 2: Install Windows Server on a Virtual Machine (VM)**

Proxmox is a powerful open-source virtualization platform that can manage virtual machines (VMs) and containers. Here’s how to create a virtual machine in Proxmox:

1. Log into Proxmox Web Interface:

   -Open your browser and go to https://<your-proxmox-ip>:8006 to access the Proxmox web interface.
   -Log in with your Proxmox credentials.

2. Create a New Virtual Machine:

   -In the Proxmox web interface, click on the Create VM button at the top right.
   -VM ID: Leave this as the default.
   -Name: Enter a name for your VM (e.g., Windows-Server-AD).
   -OS: Select the correct ISO image (Windows Server 2022 or 2019) that you downloaded in the previous step. If it’s not listed, upload the ISO to your Proxmox server first.
   -System: Leave the default settings (BIOS/UEFI, etc.).
   -Disks: Choose the disk size for your VM (at least 40 GB recommended).
   -CPU: Allocate at least 2 CPU cores for the VM.
   -Memory: Allocate at least 2 GB of RAM for the VM.
   -Network: Choose the default virtio network model.

3. Start the VM:

   -Once the VM is created, select the VM from the left sidebar, click Start, and then open the Console to begin the installation process.
