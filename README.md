# Windows-NTP-Notes

This is just a collection of notes on how to configure NTP time sources in Windows.

In Powershell:-

Query the time source.

**w32tm /query /source **


Set an NTP time source

**Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\w32time\Parameters" -Name "NtpServer" -Value "ntp.tpg.com.au,0x8" **

Restart-Service w32Time 

**w32tm /resync**


Query the status of the time service

**w32tm /query /status **

# in AD Domain Environment, [Type] is generally set to [NT5DS]
PS C:\Users\Administrator> **(Get-Item -Path "HKLM:\SYSTEM\CurrentControlSet\Services\w32time\Parameters").GetValue("Type") **
NT5DS

# on a Domain Controller of Forrest Root,
# if target is [Local CMOS Clock], change [Type] to [NTP]
# next, change to NTP server with the same way in [1] section
PS C:\Users\Administrator> **Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\w32time\Parameters" -Name "Type" -Value "NTP" **
