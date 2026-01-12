# Rotate Windows LAPS password

Windows LAPS automatically rotates the password according to settings in the policies. If the password needs to be rotated before the maximum password age is reached, this must be done manually in the Microsoft Intune admin center https://intune.microsoft.com 

Open Devices > By platform > Windows

<img width="1496" height="802" alt="image" src="https://github.com/user-attachments/assets/be63a48e-b821-4512-be91-1cc0d5e87e4a" />

Select the device on which the password for the local administrator is to be rotated.

<img width="1487" height="794" alt="image" src="https://github.com/user-attachments/assets/c1039fe2-3ce8-47ce-9ba9-0fea306b4cb9" />

In the Overview, click on the three dots and select Rotate local admin password.

<img width="1495" height="804" alt="image" src="https://github.com/user-attachments/assets/94c931fa-e304-461e-89e5-fb73e2368c11" />

Confirm the message with Yes. At the next restart Windows LAPS rotates the password on this device.

<img width="1491" height="793" alt="image" src="https://github.com/user-attachments/assets/6adc4039-1400-49af-b714-f2149e106a14" />

