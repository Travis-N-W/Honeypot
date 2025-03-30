# Honeypot (Azure)

## 📸 Project Screenshots
Click [here](https://github.com/Travis-N-W/Honeypot/tree/main/screenshots) to view all screenshots.

## Brief Objective
The objective of this project was to simulate a honeypot in the cloud by configuring an Azure virtual machine and integrating it with Microsoft Sentinel for security monitoring. The goal was to simulate a brute force attack via RDP and detect successful sign-ins using a custom alert rule. By setting up a Log Analytics workspace and using the Windows Security Events connector, any attack attempts were captured, allowing for scheduled monitoring and alerting within Microsoft Sentinel.

## Skills Learned
- Azure Virtual Machine Management
- Microsoft Sentinel Integration
- Data Connector Configuration
- Custom Alert Rule Creation
- Security Event Simulation and Monitoring

## Tools Used
- Microsoft Azure

## Steps

### 1. Creating an Azure Account
- Visit the [Azure portal](https://azure.microsoft.com/) and create a free account.
- Utilize the free $200 credits provided by Azure.

### 2. Setting Up the Virtual Machine
- In the Azure portal, navigate to **Virtual Machines**.
- Select **Azure Virtual Machine with preset configuration**.
- Configure the VM with the following parameters:
  - **Resource Group:** `myvm_group`
  - **VM Name:** `myVM`
  - **Region:** `East US`
  - **Availability Options:** Zone 2
  - **Image:** Windows 10 Pro, version 22H2 - x64 Gen2
  - **Size:** Standard_D4s_v3 (4 vCPUs, 16 GiB memory)
  - **Administrator Account:**
    - Username: `Example`
    - Password: `Example123`
  - **Inbound Ports:** Enable RDP (3389) temporarily

### 3. Deploying the Virtual Machine
- Review the configuration and click **Create**.
- Wait a few minutes for the VM to be successfully deployed.

### 4. Setting Up Microsoft Sentinel
- Navigate to **Microsoft Sentinel**.
- Create a **Log Analytics Workspace** within the same resource group (`myvm_group`).
- Name the instance `TW-LogAnalytics` and ensure it's in the `East US` region.

### 5. Configuring the Log Analytics Workspace
- Connect the virtual machine to the workspace for event log forwarding.
- This allows Microsoft Sentinel to collect security-related data from the VM.

### 6. Setting Up the Data Connector
- Install the **Windows Security Events** connector from the Content Hub.
- Use the **Azure Monitoring Agent (AMA)** to collect security logs.

### 7. Configuring Data Collection Rules
- Open the connector page and create a new **Data Collection Rule**.
- Select the resource group `myvm_group` and VM `myVM`.
- Choose the `All Security Events` option to stream comprehensive event logs.

### 8. Creating a Custom Alert Rule in Microsoft Sentinel
- Navigate to **Logs** in Sentinel and create a new alert rule.
- Configure the following parameters:
  - **Rule Name:** `Successful Local Sign-Ins`
  - **Severity:** Medium
  - **MITRE ATT&CK Tactic:** Initial Access

### 9. Configuring the Rule Query
- Set the rule query to execute every **5 minutes**.
- Lookup period: **5 minutes**.
- Alert threshold: greater than 0.
- Group similar events into a single alert to reduce noise.

### 10. Testing the Honeypot with a Simulated Brute Force Attack
- Connect to the virtual machine via RDP.
- After a successful login attempt, the **alert rule triggers an incident**.
- The incident appears in Microsoft Sentinel, confirming the honeypot is working as intended.
