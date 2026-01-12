# Retrieve Windows LAPS password history

PowerShell allows to read the password history of the local administrator account. This can be useful if a device is restored to a previous restore point and thus the current local administrator password is not valid. With PowerShell, a maximum of the last three local administrator passwords are displayed.

To be able to retrieve the Windows LAPS password history, the PowerShell cmdlet Microsoft.Graph is required.

    Install-Module microsoft.graph -Scope AllUsers

Connect to Microsoft Graph and set the two permissions Device.Read.All and DeviceLocalCredential.Read.All.

    Connect-MgGraph -Scope "Device.Read.All","DeviceLocalCredential.Read.All"

The Get-LapsAADPassword cmdlet displays the last three local administrator passwords.
Replace parameter -DeviceIDs with the device name.

    Get-LapsAADPassword -DeviceIds OMUVWSWX001 -IncludePasswords -AsPlainText -IncludeHistory

The output shows the last three local administrator passwords (1-3) in plain text with the respective expiration date.

<img width="980" height="559" alt="image" src="https://github.com/user-attachments/assets/f9f069a0-c6ae-41a3-a508-a88e6493d3b0" />
