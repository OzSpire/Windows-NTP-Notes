# Windows-NTP-Notes

This is just a collection of notes on how to configure NTP time sources in Windows.

In elevated Powershell:-

Query the time source.

>w32tm /query /source


Set an NTP time source

>Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\w32time\Parameters" -Name "NtpServer" -Value "ntp.tpg.com.au,0x8"

>Restart-Service w32Time

>w32tm /resync


Query the status of the time service

>w32tm /query /status

in AD Domain Environment, [Type] is generally set to [NT5DS] for domain members.
>(Get-Item -Path "HKLM:\SYSTEM\CurrentControlSet\Services\w32time\Parameters").GetValue("Type")

>NT5DS

on a Domain Controller of Forrest Root,
if target is [Local CMOS Clock], change [Type] to [NTP]
next, change to NTP server with the same way in [1] section
>Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\w32time\Parameters" -Name "Type" -Value "NTP"



## NT5DS

`NT5DS` stands for “Net Time 5 Directory Service.” It’s a mode of time synchronization utilized by the Windows Time Service (`w32time`) on computers that are members of an Active Directory (AD) domain.

When a Windows machine is configured to use `NT5DS` mode (which is the default for domain-joined computers), it will synchronize its time hierarchically with the domain’s structure.

Windows Default options:
NTP – Used on computers that are not joined to a domain.
NT5DS – Used on computers that are joined to a domain.

Pretty straight forward. Windows has two default options. Computers NOT joined to the DOMAIN will use NTP and Computer joined to the DOMAIN will use NT5DS, NOT NTP. NT5DS is what every Domain device uses. It’s default. It’s a time service that synchronizes the entire domain hierarchy.

