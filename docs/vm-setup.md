
# VirtualBox Setup for Windows 11 VM

### 1. Download Windows 11 ISO
- Download the official Windows 11 ISO from the [Microsoft website](https://www.microsoft.com/software-download/windows11).

### 2. Create a New Virtual Machine in VirtualBox
1. **Open VirtualBox** and click on **New** to create a new virtual machine.
2. **Name the VM**: Enter the name of the VM (e.g., "Windows 11"), choose the installation folder, and set **Type** to "Microsoft Windows" and **Version** to "Windows 11 (64-bit)".
3. **Allocate CPU and Memory**:
   - **CPU**: Set to 5 CPUs (5 cores) under the **System** tab > **Processor** tab.
   - **Memory (RAM)**: Allocate at least 4 GB of RAM (recommended 8 GB or more depending on your host).
4. **Create Virtual Hard Disk**:
   - Choose **Create a virtual hard disk now**.
   - **Hard disk size**: Set it to 100 GB (or more if needed).
   - Select **VDI** (VirtualBox Disk Image) as the hard disk type.
   - Choose **Dynamically allocated** for better disk space management.
5. **Attach Windows 11 ISO**:
   - In **Settings** > **Storage**, click on the **Empty** optical drive and select **Choose a disk file**. Browse to the Windows 11 ISO that you downloaded.

### 3. Network Adapter Settings
Ensure that the VM network adapter is set to **Bridged Adapter**, which will allow your VM to be on the same network as your host machine.

1. Go to **Settings** of the VM.
2. Navigate to **Network** > **Adapter 1**.
3. Set **Attached to** to **Bridged Adapter**.
4. Under **Name**, select the network interface card of your host machine (either **Ethernet** or **Wi-Fi** depending on your connection).
5. Click **OK** to save the settings.
6. **Restart the VM**.

### 4. Installing Windows 11 on VirtualBox
1. Start the VM. You should see the Windows 11 installation process booting from the ISO.
2. Follow the on-screen instructions to install Windows 11.
   - Choose your language, time, and keyboard preferences.
   - Enter the product key or click **I don’t have a product key** to install without it.
   - Select the **Windows 11 edition**.
   - Choose **Custom Installation** and install Windows 11 on the 100 GB virtual hard disk.
3. After installation, complete the setup process.

### Post-Installation Setup & Network Connectivity Troubleshooting

#### 1. Enable ICMP (Ping) in Windows Firewall
By default, Windows blocks ping (ICMP) requests, which can prevent you from checking the VM's connectivity from the host machine.

To allow incoming ICMP requests:

1. Open **Control Panel** inside the virtual machine.
2. Navigate to **System and Security** > **Windows Defender Firewall**.
3. Click on **Advanced settings** in the left sidebar.
4. In the **Inbound Rules** section, scroll down and find **File and Printer Sharing (Echo Request - ICMPv4-In)**.
5. **Enable ICMP (Ping)**:
   - Right-click on the rule and click **Enable Rule**. You may see multiple entries for different profiles (e.g., private, public), enable each one.
6. **Save changes** and close the firewall settings.

#### 2. Disable Third-Party Antivirus/Firewall Temporarily
If you are using third-party antivirus or firewall software on either your host or your VM, it may block ICMP or other network communication between the two. Temporarily disable it and try pinging the VM from your host.

#### 3. Check IP Address of the VM
Ensure that both your host and virtual machine are on the same subnet.

- **Find VM IP Address**:
   - Inside the virtual machine, open **Command Prompt** and type:
     ```bash
     ipconfig
     ```
   - Note the **IPv4 Address** of the VM, which should look like `192.168.29.32`.

- **Check Host Machine IP**:
   - On your host machine, open **Command Prompt** and type:
     ```bash
     ipconfig
     ```
   - Ensure that the IPv4 address of the host machine is on the same network as the VM (e.g., `192.168.29.x`). If they are on different subnets, they won’t be able to communicate.

#### 4. Ping the VM from the Host Machine
Once the VM has an IPv4 address and the firewall is configured correctly:

1. Open **Command Prompt** on your host machine.
2. Type the following command, replacing `192.168.29.32` with your VM’s actual IP:
   ```bash
   ping 192.168.29.32
   ```
3. If successful, you should see replies from the VM, confirming network connectivity.

#### 5. Additional Testing
- If ping works, you can try other services like **RDP (Remote Desktop)**, **SSH**, or accessing any web services running on the VM to ensure full connectivity.
