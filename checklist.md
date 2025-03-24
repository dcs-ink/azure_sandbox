Here’s a **structured checklist** based on your Azure environment plan, broken down by component and deployment phase:

---

### **1. Networking Setup**
- [ ] **Create Azure Virtual Network (VNet)**  
  - Name: `vnet-main`  
  - Address space: `10.0.0.0/16`  
  - Subnet: `lan-subnet` (`10.0.1.0/24`)  
- [ ] **Deploy pfSense VM**  
  - Size: `B2s` (2 vCPUs, 4GB RAM)  
  - Assign public IP to WAN interface  
  - Configure LAN interface: `10.0.1.1/24`  
  - Enable IP forwarding in Azure NIC settings  
- [ ] **Disable Azure DHCP** for `lan-subnet` (if using ISC DHCP)  

---

### **2. DHCP & DNS**
- [ ] **Deploy ISC DHCP VM**  
  - Size: `B1s` (1 vCPU, 1GB RAM)  
  - OS: Ubuntu 22.04 LTS  
  - Install ISC DHCP:  
    ```bash
    sudo apt update && sudo apt install isc-dhcp-server -y
    ```  
  - Configure `/etc/dhcp/dhcpd.conf`:  
    ```plaintext
    subnet 10.0.1.0 netmask 255.255.255.0 {
      range 10.0.1.100 10.0.1.200;
      option routers 10.0.1.1;
    }
    ```  
  - Start service:  
    ```bash
    sudo systemctl start isc-dhcp-server
    ```  

---

### **3. Work VM**
- [ ] **Deploy Work VM**  
  - OS: Windows 11/10 or Ubuntu  
  - Size: `B2s` (light) or `B4ms` (heavy workloads)  
  - Private IP: `10.0.1.10`  
  - Configure default gateway: `10.0.1.1`  

---

### **4. Database & Storage**
- [ ] **Postgres Deployment**  
  - **Option 1 (VM)**:  
    - Size: `D2s v3` (2 vCPUs, 8GB RAM)  
    - Install:  
      ```bash
      sudo apt install postgresql postgresql-contrib
      ```  
    - Configure data directory on attached SSD  
  - **Option 2 (PaaS)**:  
    - Create "Azure Database for PostgreSQL" (Flexible Server)  
- [ ] **Storage Server**  
  - Size: `E4s v3` (4 vCPUs, 32GB RAM) for large storage  
  - Install Samba/NFS:  
    ```bash
    sudo apt install samba nfs-kernel-server
    ```  
  - Mount Azure Blob Storage (optional)  

---

### **5. Identity & Domain**
- [ ] **Entra ID (Azure AD)**  
  - Enable "Azure AD Join" for VMs (no VM required)  
- [ ] **Windows AD (Optional)**  
  - Deploy Windows Server 2022 VM (`B4ms`)  
  - Promote to Domain Controller using `Add-WindowsFeature AD-Domain-Services`  

---

### **6. Email & Communication**
- [ ] **Set up Microsoft 365**  
  - Purchase license (Exchange Online)  
  - Configure MX records in DNS  
- [ ] **Self-Hosted Email (Not Recommended)**  
  - Deploy Mailcow VM (`B2s` minimum)  
  - Configure firewall rules for SMTP/IMAP  

---

### **7. Security & Validation**
- [ ] **Network Security Groups (NSGs)**  
  - Restrict SSH/RDP to your public IP  
  - Allow internal traffic within `lan-subnet`  
- [ ] **Test Connectivity**  
  - Verify DHCP leases: `cat /var/lib/dhcp/dhcpd.leases`  
  - Ping between VMs: `ping 10.0.1.10`  
  - Validate internet access via pfSense  

---

### **8. Automation (Optional)**
- [ ] **Azure CLI Scripts** for repeatable deployments  
- [ ] **Backup Configuration**  
  - Enable Azure Backup for critical VMs  
  - Schedule PostgreSQL dumps (if using VM)  

---

### **Diagram Reference**
Attach this to your checklist:  
```plaintext
[Internet]
│
├─ [pfSense VM] → [Azure Public IP]
│   ├─ LAN (10.0.1.0/24)
│   │   ├─ Work VM (10.0.1.10)
│   │   ├─ ISC DHCP (10.0.1.20)
│   │   ├─ Postgres (10.0.1.30)
│   │   └─ Storage (10.0.1.40)
│
├─ [Entra ID]
└─ [Microsoft 365]
```

---

### **Post-Deployment**
- [ ] Document all IPs and credentials in a password manager  
- [ ] Set up monitoring (Azure Monitor or Grafana)  

