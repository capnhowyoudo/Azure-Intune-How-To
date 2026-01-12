# :construction: Under Construction :construction:

# Windows LAPS in Microsoft Intune
Windows LAPS (Local Administrator Password Solution) offers a streamlined, secure, and centralized way to manage local administrator passwords via Microsoft Intune. Each device is assigned a unique, time-limited local administrator password, which is automatically managed for expiration and rotation. The passwords are securely stored in either Microsoft Entra ID (formerly Azure Active Directory) or in a local Active Directory.

Centralized management of these passwords simplifies both control and oversight. Time-bound password rotations minimize their exposure risk, while strict access controls ensure only authorized users can retrieve the passwords. This overall approach enhances the security of the network.

This guide provides instructions on configuring Windows LAPS in Microsoft Intune to store local administrator passwords in Microsoft Entra ID.

# Operating Systems

The following fully patched operating systems support Windows LAPS:

  - Windows 11: Current supported version (recommended: Version 24H2, as it offers support for automatic administrator account management)
  - Windows 10: Current supported version
  - Windows Server 2022
  - Windows Server 2019

# Licensing

  - Microsoft Entra ID Free or higher
    (when using administrative units Microsoft Entra ID P1 or higher)
  - Microsoft Intune Plan 1 or higher

# Roles

A role with the microsoft.directory/deviceLocalCredentials/password/read permission is required to retrieve the local administrator password. This permission is part of the following roles:

  - Global Administrator
  - Intune Administrator
  - Cloud Device Administrator

# Enable Windows LAPS

Windows LAPS is activated through the Microsoft Entra ID admin center at https://portal.azure.com/

Open Identity > Devices > All devices > Device Settings and enable the feature Enable Microsoft Entra Local Administrator Password Solution (LAPS)

<img width="1499" height="808" alt="image" src="https://github.com/user-attachments/assets/059323d8-da9f-40c8-b664-1499fa25b58e" />

# Configure Windows LAPS

The policy for Windows LAPS is created in the Microsoft Intune admin center https://intune.microsoft.com/

Open Endpoint Security > Manage > Account protection and create a new policy with Create Policy

<img width="1512" height="814" alt="image" src="https://github.com/user-attachments/assets/094cc3cc-8410-4d2d-9af3-2b92dfeb00d1" />

Select Platform Windows (1), Profile Local admin password solution (Windows LAPS) (2) create the profile with Create (3).

<img width="1505" height="808" alt="image" src="https://github.com/user-attachments/assets/2bef5cb3-321c-4a3f-8006-3b049995daca" />

Name the profile (e.g.,LAPS) and click Next.

<img width="1460" height="875" alt="image" src="https://github.com/user-attachments/assets/5bb679b7-b001-4469-96f9-70204fa5c44b" />

Configuring Windows LAPS:
The following Windows LAPS configuration serves as a suggestion and can be customized and extended as needed.

| Setting  | Value |
| ------------- | ------------- |
| Backup Directory  | 	Backup the password to Azure AD only  |
| Password Age Days  | 30  |
| Administrator Account Name  | Configured  | 
| Administrator Account Name  | LAPS  | 
| Password Complexity  | Large Letters + Small Letters + Numbers + Special Characters (Default) |
| Passphrase Length  | 	14  |
| Password Length | Configured |

<img width="1368" height="876" alt="image" src="https://github.com/user-attachments/assets/294acba6-d856-41c8-9596-f95ffb7f96f6" />

Create custom scope tags based on individual requirements.

<img width="1272" height="877" alt="image" src="https://github.com/user-attachments/assets/ce2ddbe6-e81d-4ffd-bcb3-349efdd5fab6" />

Assignment to devices can be customized according to specific requirements. I am using a group i created. You can use all devices or a group you have created. 

<img width="1495" height="876" alt="image" src="https://github.com/user-attachments/assets/0bba846f-8db3-4144-be18-773e3d12dde2" />

Review the profile settings and complete the creation by clicking Save.

<img width="1241" height="874" alt="image" src="https://github.com/user-attachments/assets/7be47067-6fe6-45fa-bc67-49dfb1999a44" />

After a short time, the new profile for Windows LAPS is created.

<img width="1913" height="853" alt="image" src="https://github.com/user-attachments/assets/8491f291-1af1-449b-a3ea-60b3419fd1b2" />



