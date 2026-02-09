# How to migrate AAD Connect to a new server 

In this guide, I will walk you through migrating your current settings to a new Windows Server 2016. You can follow these same steps whenever you need to move Microsoft Entra Connect (formerly Azure AD Connect) to a different server.
Migration Overview

The high-level process involves:

1. Exporting the current configuration from your existing server.

2. Installing the latest version of the tool on the new Windows Server 2016.

3. Importing your settings, enabling Staging Mode, and performing a test sync.

4. Decommissioning the old server by uninstalling the software.

5. Promoting the new server by disabling Staging Mode to go live.

On your primary server, launch the Azure AD Connect tool. Navigate to the tasks menu and choose View or export current configuration to save your settings."

<img width="874" height="618" alt="image" src="https://github.com/user-attachments/assets/231e024d-a73c-4099-9e4e-d410d0f8787b" />

Click the Export Settings button

<img width="873" height="615" alt="image" src="https://github.com/user-attachments/assets/6adc1ac6-888c-4345-a03e-d1dec9d347f1" />

<img width="846" height="549" alt="image" src="https://github.com/user-attachments/assets/dde300c4-16f7-4cc0-b52e-37f5e72e592e" />

The settings will be exported as a single JSON file in C:\ProgramData\AADConnect by default. 
Copy this file to the new AAD Connect server.

Access your target Windows Server 2016/2019/2022 machine. Once logged in, download the installer from the [Microsoft site](https://www.microsoft.com/en-us/download/details.aspx?id=47594) and begin the installation process.

<img width="881" height="618" alt="image" src="https://github.com/user-attachments/assets/ca3035e2-06a8-4a4a-8915-841d64aa6e3d" />

Start the AADC installer.

<img width="879" height="616" alt="image" src="https://github.com/user-attachments/assets/3cabdce3-dd20-4e99-85b7-55a0112b5d6d" />

Select Customize so you can import the existing configuration.

<img width="875" height="617" alt="image" src="https://github.com/user-attachments/assets/5947a182-9860-48a1-9d32-9d74ca894020" />

Check Import synchronization settings, then browse to the JSON file you copied from the old server. Click Install to begin the installation.

<img width="875" height="616" alt="image" src="https://github.com/user-attachments/assets/a4b2161e-e73a-4660-ba75-5e1bfe892ae6" />

The installer guides you through the setup using the existing configuration, similar to a manual upgrade.

<img width="871" height="614" alt="image" src="https://github.com/user-attachments/assets/a453a14f-c0f6-4fd8-afe8-0f5b19b0046c" />

Check Enable staging mode, then click Install.

<img width="876" height="616" alt="image" src="https://github.com/user-attachments/assets/cbab06b8-7d51-4b78-932b-d29512fd6b48" />

The installation may take a few minutes to complete and should look like this. Once complete, click Exit.

<img width="979" height="706" alt="image" src="https://github.com/user-attachments/assets/a6a6397a-8175-40ab-8313-a055ae34dce6" />

On the new server, open Computer Management and add the domain Enterprise Admins group to the local ADSyncAdmins group to enable AAD Connect management. Log off and back on to activate the permissions.

<img width="800" height="587" alt="image" src="https://github.com/user-attachments/assets/a2d4637b-1178-44d3-bcee-800ee5d27bdd" />

The two Azure AD Connect Health Sync services, along with the Microsoft Azure AD Sync service, are now installed and running on the new server.

At this stage, complete the AAD Connect migration by removing AADC from the old server and turning off staging mode on the new server.


<img width="790" height="623" alt="image" src="https://github.com/user-attachments/assets/71ee6266-f3ed-4b1a-8afa-764fc826dc12" />

Launch the Synchronization Service Manager client from C:\Program Files\Microsoft Azure AD Sync\UIShell\miisclient.exe to verify that the initial full sync has completed on the new server.

