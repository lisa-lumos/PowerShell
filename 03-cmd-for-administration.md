# 3. Using the Command Line for Administration
## General Server Management
```powershell
# details about the current system, serial number, used memory, ...
Get-ComputerInfo

# manage Windows Server roles and features
# needs to be run over the network, on a server
# Install/Uninstall-WindowsFeature is related to this cmd
Get-WindowsFeature -ComputerName LONDC1

# To join a domain, or remove from a domain, rename a computer, ...
Add-Computer
Remove-Computer
Rename-Computer

# Restart a machine, wait for the PowerShell is up, then write "Done" to the terminal
Restart-Computer -ComputerName LONSVR1 -Wait -For PowerShell ; Write-Host "Done"

# to test the trust relationship between the machine and the domain
# check if it has lost its connection to the domain
# if its trust relationship with the domain has been severed
# if its been corrupted because the machine has been offline for a long time
# if its clock is skewed
Test-ComputerSecureChannel

# if prev cmd returns false, try to repair it
Test-ComputerSecureChannel -Repair

Get-Date
Set-Date 

Set-Timezone
Get-Timezone
```

## Networking Settings
PowerShell also includes a lot of networking commands that can replace for some traditional command line commands, such as ip-config. 

```powershell
# return the network interface info
Get-NetIPAddress

# return info about the use of the adapter
Get-NetAdapterStatistics

# DNS suffix
Get-DnsClient
Get-DnsClientServerAddress
Get-DnsServer -ComputerName LONDC1

# get all the firewall rule on the system
Get-NetFirewallRule

# ping also works here
ping LONSVR1

# older version, gives 4 errors if not exist
Test-Connection LONSVR1

# newer version, test internet access, if given no param vals
Test-NetConnection

# Test network connection of another machine
Test-NetConnection LONSVR1

# Checks if port 80 is open on another machine
Test-NetConnection LONSVR1 -Port 80

# Trace to the destination
Test-NetConnection LONSVR1 -TraceRoute

# similar to Linux nslookup, or dig command
Resolve-DnsName www.microsoft.com
```

## Overview of Hyper-V
Hyper-V is microsoft's hypervisor. 
```powershell
Get-Command -Module Hyper-V

# spin up a new vm quickly
New-VM

Get-VM
Set-VM
Start/Stop/Restart/Suspend/Resume/Checkpoint-VM
Enable-VMResourceMetering
Measure-VM

# can use pipes
Get-VM | CheckPoint-VM
Get-VM | Enable-VMResourceMetering
Get-VM | Measure-VM

# For an entire environment/data-center
Invoke-Command list {Get-VM | Measure-VM}
```

## PowerShell Comparison Operators








## Querying Active Directory



## Customizing AD Searches



## Administering Active Directory



## Introduction to Azure PowerShell


































