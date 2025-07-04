# 🌐 Three-Tier Cloud Architecture on Azure with Apache and IIS

This project demonstrates the implementation of a **secure and scalable three-tier cloud architecture** using **Microsoft Azure**, following real-world best practices. Each tier includes both **Linux** and **Windows** virtual machines configured with **Apache** and **IIS** web servers respectively.

---

## 📁 Project Overview

### 📌 Objectives

- Design a **three-tier architecture** consisting of:
  - **Web Tier**: Public-facing frontend
  - **App Tier**: Internal APIs or backend services
  - **DB Tier**: Databases or secure storage
- Deploy **two VMs per tier** (1 Linux + 1 Windows)
- Configure:
  - **Apache HTTP Server** on Linux
  - **IIS Web Server** on Windows
- Implement **Network Security Groups (NSGs)** to:
  - Allow only permitted traffic between tiers
  - Restrict internet access to Web Tier only
  - Ensure secure isolation for DB Tier

---

## 🗺️ Architecture Diagram

     +-------------------+       Internet Access Allowed
     |   Web Subnet      | <-----------------------------+
     | [Linux: Apache]   |                               |
     | [Windows: IIS]    |                               |
     +-------------------+                               |
               ↓                                          |
         Internal Access Only                             |
               ↓                                          |
     +-------------------+                                |
     |   App Subnet      |                                |
     | [Linux: Apache]   |                                |
     | [Windows: IIS]    |                                |
     +-------------------+                                |
               ↓                                          |
         Internal Access Only                             |
               ↓                                          |
     +-------------------+                                |
     |   DB Subnet       |                                |
     | [Linux: Apache]   |                                |
     | [Windows: IIS]    |                                |
     +-------------------+                                |
                                                           |
              ⛔ DB Tier cannot access App or Web tiers ---+

---

## ⚙️ Implementation Steps

### 1️⃣ Create Virtual Network & Subnets

- Create a Virtual Network (VNet): `MyVNet`
- Define address space: `10.0.0.0/16`
- Create 3 subnets:
  - `WebSubnet` → 10.0.1.0/24
  - `AppSubnet` → 10.0.2.0/24
  - `DBSubnet`  → 10.0.3.0/24

### 2️⃣ Create Network Security Groups (NSGs)

- **WebNSG**: Allows internet access, allows traffic from AppSubnet
- **AppNSG**: Allows access from WebSubnet, allows outbound to DBSubnet
- **DBNSG**: Only allows access from AppSubnet, no outbound traffic

> Attach NSGs to corresponding subnets, **not individual NICs**.

### 3️⃣ Deploy Virtual Machines

Per subnet:
- One **Ubuntu VM** (for Apache)
- One **Windows Server VM** (for IIS)

Use the **same resource group and region** for all resources.

### 4️⃣ Configure Apache on Linux VMs

```bash
# Connect using SSH
sudo apt update
sudo apt install apache2 -y
sudo systemctl enable apache2
sudo systemctl start apache2
```

Check Apache at: http://<Public-IP-of-Web-Linux-VM>

## 5️⃣ Configure IIS on Windows VMs
- Connect using RDP

- Open Server Manager

- Add role: Web Server (IIS)

- Start IIS

- Check IIS at: http://<Public-IP-of-Web-Windows-VM>

## 6️⃣ Validate Tier Communication
- Test using browser or ping:

- ✅ Web → App: Allowed

- ✅ App → Web & DB: Allowed

- ✅ DB → App/Web: ❌ Denied

- ✅ Web → Internet: Allowed

- ❌ App/DB → Internet: Denied


## LIVE DEMO : 
## Virtual Network 
![Homepage Screenshot](assets/Screenshot%202025-07-04%20141403.png)
## Subnets
![Homepage Screenshot](assets/Screenshot%202025-07-04%20141438.png)
## web-nsg-rules
![Homepage Screenshot](assets/Screenshot%202025-07-04%20141504.png)
## app-nsg-rules
![Homepage Screenshot](assets/Screenshot%202025-07-04%20141539.png)
## db-nsg-rules
![Homepage Screenshot](assets/Screenshot%202025-07-04%20141519.png)
## vm-creation-linux
![Homepage Screenshot](assets/Screenshot%202025-07-04%20141611.png)
![Homepage Screenshot](assets/Screenshot%202025-07-04%20141632.png)
## vm-creation-windows
![Homepage Screenshot](assets/Screenshot%202025-07-04%20141641.png)
## result
![Homepage Screenshot](assets/Screenshot%202025-07-04%20131335.png)
![Homepage Screenshot](assets/Screenshot%202025-07-04%20132916.png)
![Homepage Screenshot](aassets/Screenshot%202025-07-04%20133006.png)

