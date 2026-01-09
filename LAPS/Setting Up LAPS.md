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

    <#
    .SYNOPSIS
    Creates a local administrator account for use with Windows LAPS.

    .DESCRIPTION
    This script checks for the existence of a local user account named "LAPS".
    If the account does not exist, it creates the account with a temporary password,
    prevents the user from changing the password, and adds the account to the local
    Administrators group.

    The temporary password is intended to be rotated automatically by Windows
    Local Administrator Password Solution (LAPS).
    #>
    
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

1. Log into https://intune.microsoft.com
2. Select Devices > Windows

<img width="1451" height="421" alt="image" src="https://github.com/user-attachments/assets/ce2e02f3-6b36-4b5c-82a4-149ad41766ae" />

3. Select Scripts and remediations > Platform Scripts > Add

<img width="1712" height="576" alt="image" src="https://github.com/user-attachments/assets/b318a702-95eb-4896-a3aa-6e3b2ecc8e39" />

4. Create a name for example "Create Local LAPS Local Accounts" > Next

<img width="1687" height="881" alt="image" src="https://github.com/user-attachments/assets/f9ebfc69-fc57-4055-8ae1-1e5f0e9b4f2e" />

5. Browse to the location where you had saved the .ps1
    - Run this script using the logged on credentials = No
    - Enforce script signature check = No
    - Run script in 64 bit PowerShell Host = Yes
    - Next

<img width="1692" height="878" alt="image" src="https://github.com/user-attachments/assets/b8ccf2b2-712a-4ead-8bb2-ecdf13eb6fa0" />

6. Add devices by group or apply to all devices, based on your requirements > Next

<img width="1686" height="879" alt="image" src="https://github.com/user-attachments/assets/4dca5d5f-c4ed-4000-8ef3-fc6bfedb92cd" />

7. Review + Create

<img width="1687" height="872" alt="image" src="https://github.com/user-attachments/assets/ff5c6770-0aa8-437b-a255-62ee6eb3dd0d" />

8. Script will be pushed the next time the workstations sync with Azure.

<img width="1691" height="680" alt="image" src="https://github.com/user-attachments/assets/a2110142-1072-442a-9e33-c0ac22dcb9bd" />

9. You can check deployment status within the platform script

<img width="1693" height="877" alt="image" src="https://github.com/user-attachments/assets/afd71903-0c88-4cb3-b5b0-4bd22efcd958" />

# Enable Windows LAPS

Windows LAPS is activated through the Microsoft Entra ID admin center at https://portal.azure.com/

Open Identity > Devices > All devices > Device Settings and enable the feature Enable Microsoft Entra Local Administrator Password Solution (LAPS)

<img width="1499" height="808" alt="image" src="https://github.com/user-attachments/assets/059323d8-da9f-40c8-b664-1499fa25b58e" />

