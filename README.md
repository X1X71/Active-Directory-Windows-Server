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
   
   ![Screenshot 2025-01-11 at 15-25-52 pve01 - Proxmox Virtual Environment](https://github.com/user-attachments/assets/2f26e78b-bef7-4c44-bf88-9f6fea3e09bd)

   -System: Leave the default settings (BIOS/UEFI, etc.).
   -Disks: Choose the disk size for your VM (at least 40 GB recommended).
   -CPU: Allocate at least 2 CPU cores for the VM.
   -Memory: Allocate at least 2 GB of RAM for the VM.
   -Network: Choose the default network model.

4. Start the VM:

   -Once the VM is created, select the VM from the left sidebar, click Start, and then open the Console to begin the installation process.

**Step 3: Install Windows Server on the Virtual Machine**

Follow the installation wizard for Windows Server:

   -Choose your language, time zone, and keyboard settings.

   -Select Install Now and then choose the version of Windows Server (Standard, Datacenter, etc.) that corresponds to your license. In this guide we will select Standard Evaluation (Desktop Experience)

   -Proceed with the installation and set the administrator password when prompted.

   -The VM will reboot several times during the installation process.
![Screenshot 2025-01-11 at 15-35-09 pve01 - Proxmox Virtual Environment](https://github.com/user-attachments/assets/3ccfb99f-e5d5-406b-b547-20b214c13252)


After installation, log in to Windows Server with the Administrator account and the password you set during installation.

**Step 4: Install Active Directory Domain Services (AD DS)**

Now that you have Windows Server installed, let’s install the Active Directory Domain Services role.

   -Open Server Manager:
   -When you log into Windows Server, Server Manager should automatically open.

   -Add the AD DS Role:
   -In Server Manager, click on Add roles and features.
   -In the Add Roles and Features Wizard, click Next until you reach the Select Features page.
   -On the Server Roles page, scroll down and check Active Directory Domain Services.
   -Click Next and then Install.
![Screenshot 2025-01-11 at 15-48-50 pve01 - Proxmox Virtual Environment](https://github.com/user-attachments/assets/92c2f8b1-3057-48a2-8d0c-526eb48d6202)

   -Wait for the installation to finish.

**Step 5: Promote the Server to a Domain Controller**

Once the AD DS role is installed, you’ll need to promote the server to a Domain Controller.

   1.Promote the Server:
   -After the installation completes, you’ll see a notification in Server Manager. Click the yellow triangle icon, then click Promote this server to a domain controller.
![Screenshot 2025-01-11 at 15-53-02 pve01 - Proxmox Virtual Environment](https://github.com/user-attachments/assets/7adc54b1-eb30-437b-931e-0055fdf308d8)

   2.Configure the Domain:
   -In the Active Directory Domain Services Configuration Wizard, select the option Add a new forest.
   -Enter a Root domain name (e.g., example.local).
   -Set a Directory Services Restore Mode (DSRM) password. This is important for domain controller recovery.
   -Click Next and proceed with the installation.
![Screenshot 2025-01-11 at 15-53-26 pve01 - Proxmox Virtual Environment](https://github.com/user-attachments/assets/9223566f-45b3-490e-839c-0fa9caff726b)

   3.Reboot the Server:
   -The server will require a reboot to complete the promotion to a domain controller. Allow it to reboot.

**Step 6: Verify Active Directory Installation**

After the server reboots, verify that Active Directory is functioning properly:

   1.Open Active Directory Users and Computers:
   -Open the Server Manager.
   -Go to Tools > Active Directory Users and Computers.
   -You should see your domain listed on the left pane (e.g., example.local).
![Screenshot 2025-01-11 at 16-03-37 pve01 - Proxmox Virtual Environment](https://github.com/user-attachments/assets/1518ec17-952c-49ba-a041-3540b30ce649)

   2.Run DC Diagnostics:
   -Open Command Prompt and type dcdiag to check the health of the domain controller.
   -Ensure all tests pass without errors.
![Screenshot 2025-01-11 at 16-04-35 pve01 - Proxmox Virtual Environment](https://github.com/user-attachments/assets/e2f2b418-4673-4cbd-b093-604273113437)

**Step 7: Install and Configure DNS Server**

Active Directory relies on DNS to function correctly. Verify that the DNS role is properly configured:

   1.Verify DNS Configuration:
   -Open DNS Manager from Server Manager > Tools > DNS.
   -Ensure that your Forward Lookup Zone for your domain (e.g., example.local) is set up correctly.
   -DNSSEC status can be left "Not Signed" for now as this is a testing environment
![Screenshot 2025-01-11 at 16-06-34 pve01 - Proxmox Virtual Environment](https://github.com/user-attachments/assets/45ce5a77-9dd6-43d9-b647-df989074323e)

   2.Test DNS Resolution:
   -Open Command Prompt and type nslookup example.local to ensure the domain name resolves correctly.
![Screenshot 2025-01-11 at 16-11-20 pve01 - Proxmox Virtual Environment](https://github.com/user-attachments/assets/3b94b815-9354-48a9-b5ca-64aa10119dd8)

**Step 8: Create Users and Groups in Active Directory**

Now, let’s create some users and groups in Active Directory:

   1.Create Organizational Units (OUs):
   -In Active Directory Users and Computers, right-click your domain (example.local) and select New > Organizational Unit (OU).
   -Enter the name for your OU (e.g., HR, IT).
![Screenshot 2025-01-11 at 16-13-04 pve01 - Proxmox Virtual Environment](https://github.com/user-attachments/assets/aad3085d-4c99-4f07-ac6c-64bdb06396da)

   2.Create Users:
   -Right-click on an OU or the Users folder, then select New > User.
   -Enter the user’s details (e.g., first name, last name, username).
   -Set a password for the user.
![Screenshot 2025-01-11 at 16-14-17 pve01 - Proxmox Virtual Environment](https://github.com/user-attachments/assets/7f3d30c0-73f7-46e4-97d5-74b1b7f3695a)

   3.Create Groups:
   -You can create groups (e.g., Admins, Users) by right-clicking the domain or OU, then selecting New > Group.

**Step 9: Install Remote Server Administration Tools (RSAT)**

If you want to manage your Active Directory remotely, you’ll need to install RSAT on a Windows client machine.

   1.On a Windows 10/11 machine, open Settings and search for "Optional features"
   2.Click Add an optional feature, search for RSAT, and install the RSAT: Active Directory Domain Services and Lightweight Directory Tools feature.
   3.If unable to find RSAT under optional features, open PowerShell as Administrator and run the following command:
```
Get-WindowsCapability -Name RSAT* -Online | Add-WindowsCapability -Online
```
![Screenshot 2025-01-11 at 16-19-58 pve01 - Proxmox Virtual Environment](https://github.com/user-attachments/assets/22043b1c-b602-40f9-96c1-c290c0dc4d51)

   4.Confirm installation by searching "Active Directory Users and Computers" in the Start Menu

**Step 10: Conclusion and Next Steps**

Congratulations, you’ve successfully installed and configured Active Directory on Windows Server inside a Proxmox VM! With Active Directory, you can manage users, groups, and policies in a centralized manner.


