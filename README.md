# Kubernetes-Azure-Automation-Lab
This repository contains automation scripts and resources for setting up and managing Kubernetes clusters on Azure. It includes examples for automating cluster scaling and monitoring using Azure Automation and Kubernetes tools.
---

### **Step 1: Set Up the Azure and Kubernetes Environment**

1. **Create an Azure Account**  
   ![Azure](https://img.shields.io/badge/Microsoft%20Azure-0089D6?logo=microsoft-azure&logoColor=white)  
   Visit [Azure Free Account](https://azure.microsoft.com/free/) to sign up.  
   **Result**: A free Azure account is created, allowing you to provision cloud resources. 

2. **Install Azure CLI**  
   ![Azure CLI](https://img.shields.io/badge/Azure_CLI-0078D4?logo=azuredevops&logoColor=white)  
   [Follow the Azure CLI Installation Guide](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).  
   **Result**: Azure CLI is installed, enabling command-line management of Azure resources.

3. **Create an Azure Kubernetes Service (AKS) Cluster**  
   ![Kubernetes](https://img.shields.io/badge/Kubernetes-326ce5.svg?&logo=Kubernetes&logoColor=white)  
   Use the following commands:
   ```bash
   az group create --name myResourceGroup --location eastus
   az aks create --resource-group myResourceGroup --name myAKSCluster --node-count 1 --enable-addons monitoring --generate-ssh-keys
   az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
   ```
   **Result**: An AKS cluster is created and configured.

4. **Verify the Kubernetes Cluster**  
   Run:
   ```bash
   kubectl get nodes
   ```
   **Result**: Displays the Kubernetes nodes, confirming cluster status.

### **Step 2: Automate with Azure Automation**

1. **Set Up Azure Automation**  
   ![Azure Automation](https://img.shields.io/badge/Azure_Automation-0089D6?logo=microsoft-azure&logoColor=white)  
   [Create an Azure Automation Account](https://docs.microsoft.com/en-us/azure/automation/automation-create-standalone-account).  
   **Result**: Automation account ready for task management.

2. **Create and Run a PowerShell Runbook**  
   ![PowerShell](https://img.shields.io/badge/PowerShell-5391FE?logo=powershell&logoColor=white)  
   Example:
   ```powershell
   az aks scale --resource-group myResourceGroup --name myAKSCluster --node-count 3
   ```
   **Result**: Automated scaling of Kubernetes cluster.

### **Additional Resources**

- **Access the Full Documentation**: [Google Drive Link](https://drive.google.com/drive/folders/1f_ra2VRKkJ7RtSDZSPI7GixBSYPTf-c0)
- **Access My CV/Resume**: [Resume](https://drive.google.com/drive/folders/12lZo1FrKTeERD6kbGwsSEbUYPLx0QrCp)
