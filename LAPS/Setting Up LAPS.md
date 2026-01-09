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

# Create New Local Administrator Account

It is recommended to create a custom local administrator account for workstations managed by Windows LAPS, while keeping the built-in "Administrator" account disabled. Here's why:

1. **Security Best Practice**: The built-in "Administrator" account has a known Relative Identifier (RID) of 500, making it a predictable target for attackers. By creating a new account with a unique, randomized name, you add an extra layer of security through obfuscation, making it more difficult for automated attacks to target the account.

2. **Account Lockout Policy**: By default, the built-in "Administrator" account is exempt from the account lockout policy. Using a custom account allows you to apply lockout policies, helping to protect against brute-force attacks.

3. **Improved Management**: A dedicated, custom account makes it easier to differentiate between administrative accounts and other domain or standard user accounts, simplifying auditing and monitoring processes.

Save the following powershell script as $${\color{blue}localLAPsAdminAccountCreation.ps1}$$

>:information_source: Password can be changed in the PasswordString.
>Password will be rotated with LAPS

    $Username = "LAPS"
    $AdminGroup = "Administrators"

    # Temporary password (will be rotated by LAPS)
    $Password = ConvertTo-SecureString "TempP@ssw0rd!ChangeMe" -AsPlainText -Force

    if (-not (Get-LocalUser -Name $Username -ErrorAction SilentlyContinue)) {
    New-LocalUser `
        -Name $Username `
        -Password $Password `
        -FullName "LAPS Local Admin Account" `
        -Description "Local Administrator account managed by LAPS" `
        -UserMayNotChangePassword
    }

    Add-LocalGroupMember -Group $AdminGroup -Member $Username -ErrorAction SilentlyContinue

# Enable Windows LAPS

Windows LAPS is activated through the Microsoft Entra admin center at https://portal.azure.com/

Open Identity > Devices > All devices > Device Settings and enable the feature Enable Microsoft Entra Local Administrator Password Solution (LAPS)

<img width="1499" height="808" alt="image" src="https://github.com/user-attachments/assets/059323d8-da9f-40c8-b664-1499fa25b58e" />

