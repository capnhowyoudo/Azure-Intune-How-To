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


