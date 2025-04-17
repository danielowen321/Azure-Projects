# deploy-azure-vm

This project demonstrates how to deploy and configure a basic Azure Virtual Machine (VM) in a **Resource Group** using the **Azure Portal**, with steps for SSH key management, networking, and custom script configuration.

---

## Learning Objectives

- Create and manage Azure Virtual Machines using the Azure Portal
- Configure SSH key-based authentication for secure VM access
- Set up a Virtual Network (VNet) and Subnet for networking
- Automate configuration using custom scripts during VM creation
- Understand VM management and monitoring tools in Azure

---

## Tools & Technologies

- [Azure Portal](https://portal.azure.com/)
- [Azure CLI](https://learn.microsoft.com/en-us/cli/azure/)
- SSH (for connecting to VM)
- Custom scripts for automation and configuration

---

### 🔹 **Step 1: Create a Resource Group**

- **Name:** `VirtualMachineDeployment`  
- **Region:** `Australia Southeast`  

**Instructions:**
1. In the [Azure Portal](https://portal.azure.com/), navigate to **Resource Groups**.
2. Click **+ Add**.
3. Set the **Resource Group Name** to `VirtualMachineDeployment`.
4. Choose **Region** as `Australia Southeast`.
5. Click **Review + Create**, then **Create**.

**Screenshot to Capture:**
- **Resource Group Creation Page** showing the name (`VirtualMachineDeployment`) and region (`Australia Southeast`) selected. This should show the configuration before you click "Create."

---

### 🔹 **Step 2: Create a Virtual Network (VNet)**

- **Name:** `VM_VNet`  
- **Address space:** `10.0.0.0/16`  
- **Subnet:** `10.0.1.0/24`  
- **Region:** `Australia East`

**Instructions:**
1. In the [Azure Portal](https://portal.azure.com/), go to **Create a resource**.
2. In the search box, type **Virtual Network** and select **Virtual Network**.
3. Click **Create**.
4. Fill in the following details:
   - **Subscription:** Choose your subscription.
   - **Resource Group:** Select `VirtualMachineDeployment` (the one you created in Step 1).
   - **Name:** `VM-1-vnet`
   - **Region:** `Australia East`
   - **Address space:** `10.0.0.0/16`
   - **Subnet:** Click **+ Add subnet** and enter `10.0.1.0/24` as the subnet address range.
5. Click **Review + Create**, then **Create**.

**Screenshot to Capture:**
- **Virtual Network Creation Page** showing the name (`VM_VNet`), address space (`10.0.0.0/16`), and subnet (`10.0.1.0/24`) selected. You should also see **Australia Southeast** as the region.
---

### 🔹 **Step 3: Create a Virtual Machine (VM)**

- **Name:** `VM-1`  
- **Image:** Ubuntu 20.04 LTS  
- **Size:** B1s  
- **Authentication:** SSH public key  
- **VNet/Subnet:** `VM-1-vnet`  
- **Region:** `East`

**Instructions:**
1. In the [Azure Portal](https://portal.azure.com/), go to **Create a resource**.
2. In the search box, type **Virtual Machine** and select **Virtual Machine**.
3. Click **Create**.
4. Fill in the following details:
   - **Subscription:** Choose your subscription.
   - **Resource Group:** Select `VirtualMachineDeployment` (the one you created in Step 1).
   - **VM Name:** `VM-1`
   - **Region:** `Australia East`
   - **Image:** Select **Ubuntu 20.04 LTS**
   - **Size:** Choose **B1s** (small instance for testing purposes).
   - **Authentication Type:** Select **SSH public key**.
   - **Public Key:** Paste your **SSH public key** (generated using `ssh-keygen`).
5. Under **Networking**, choose:
   - **Virtual Network:** Select `VM-1-vnet`.
   - **Subnet:** Choose the `default` subnet you created earlier (`10.0.1.0/24`).
6. Click **Review + Create**, then **Create**.

**Screenshot to Capture:**
- **Virtual Machine Creation Page** showing the selected image (Ubuntu 20.04 LTS), size (B1s), authentication method (SSH public key), and the **Networking** section showing the `VM-1-vnet` and subnet `10.0.1.0/24`.

---

### 🔹 **Step 4: Install Apache2 Using a Custom Script (Post-Deployment)**
What I did:

After the VM was successfully deployed, I used the **Extensions + Applications** feature in the Azure Portal to run a custom Bash script that installs and starts the **Apache2 web server.**

Script used:

<pre> <code> #!/bin/bash apt update apt install apache2 -y systemctl start apache2 </code> </pre>

---

### 🔹 **Step 5: Configure NSG Rules for Inbound Access**
What I did:

Added a **Network Security Group (NSG)** inbound rule to allow **HTTP (port 80)** traffic so the Apache web server is accessible publicly.

**Configuration:**

**Port: 80 (HTTP)**

**Protocol: TCP**

**Priority: 1001**

**Action: Allow**

Action:

**Go to VM-1 in the Azure Portal.**

**Navigate to Networking → Inbound port rules.**

**Click + Add inbound port rule.**

**Enter the configuration above.**

**Click Add to save the rule.**

