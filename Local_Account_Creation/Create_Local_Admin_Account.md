# Create New Local Administrator Account

1. **Security Best Practice**: The built-in "Administrator" account has a known Relative Identifier (RID) of 500, making it a predictable target for attackers. By creating a new account with a unique, randomized name, you add an extra layer of security through obfuscation, making it more difficult for automated attacks to target the account.

2. **Account Lockout Policy**: By default, the built-in "Administrator" account is exempt from the account lockout policy. Using a custom account allows you to apply lockout policies, helping to protect against brute-force attacks.

3. **Improved Management**: A dedicated, custom account makes it easier to differentiate between administrative accounts and other domain or standard user accounts, simplifying auditing and monitoring processes.

Save the following powershell script as $${\color{blue}localAdminAccountCreation.ps1}$$

>:information_source: Password can be changed in the PasswordString.
>Password will be rotated with LAPS

    <#
    .SYNOPSIS
    Creates a local administrator account.

    .DESCRIPTION
    This script checks for the existence of a local user account.
    If the account does not exist, it creates the account with a temporary password,
    prevents the user from changing the password, and adds the account to the local
    Administrators group.
    #>

    # Generic local admin username
    $Username   = "LocalAdmin"
    $AdminGroup = "Administrators"

    # Temporary password
    $Password = ConvertTo-SecureString "TempP@ssw0rd!ChangeMe" -AsPlainText -Force

    # Create the user if it does not exist
    if (-not (Get-LocalUser -Name $Username -ErrorAction SilentlyContinue)) {
    New-LocalUser `
        -Name $Username `
        -Password $Password `
        -FullName "Local Administrator Account" `
        -Description "Generic local administrator account" `
        -UserMayNotChangePassword `
        -PasswordNeverExpires
    }

    # Add user to Administrators group
    Add-LocalGroupMember -Group $AdminGroup -Member $Username -ErrorAction SilentlyContinue


1. Log into https://intune.microsoft.com
2. Select Devices > Windows

<img width="1451" height="421" alt="image" src="https://github.com/user-attachments/assets/ce2e02f3-6b36-4b5c-82a4-149ad41766ae" />

3. Select Scripts and remediations > Platform Scripts > Add

<img width="1712" height="576" alt="image" src="https://github.com/user-attachments/assets/b318a702-95eb-4896-a3aa-6e3b2ecc8e39" />

4. Create a name for example "Local Admin Account Creation" > Next

<img width="1583" height="879" alt="image" src="https://github.com/user-attachments/assets/257741bd-d493-4fb4-b598-66a1e0617df1" />

5. Browse to the location where you had saved the .ps1
    - Run this script using the logged on credentials = No
    - Enforce script signature check = No
    - Run script in 64 bit PowerShell Host = Yes
    - Next

<img width="1485" height="878" alt="image" src="https://github.com/user-attachments/assets/2444c811-f87a-404b-8a03-fad9a2183c82" />

6. Add devices by group or apply to all devices, based on your requirements > Next

<img width="1686" height="879" alt="image" src="https://github.com/user-attachments/assets/4dca5d5f-c4ed-4000-8ef3-fc6bfedb92cd" />

7. Review + Create

<img width="1538" height="878" alt="image" src="https://github.com/user-attachments/assets/ade516d2-0aba-43af-861f-b4ff387d7db0" />

8. Script will be pushed the next time the workstations sync with Azure.

<img width="1907" height="877" alt="image" src="https://github.com/user-attachments/assets/ae827faa-38d5-47d0-8b28-18d704b7ddab" />

9. You can check deployment status within the platform script

<img width="1576" height="879" alt="image" src="https://github.com/user-attachments/assets/b2a9d2ea-27c7-4d18-a6e8-ac70830f80be" />
