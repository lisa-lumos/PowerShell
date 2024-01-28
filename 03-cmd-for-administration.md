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
For example:
- `-eq`
- `-ne`
- `-gt`
- `-lt`
- `-ge`
- `-le`
- `-like`: can be used for partial match
- `-notlike`: can be used for partial match

```powershell
5 -eq 4            # False
"TEST" -like "TE*" # True

# PS strings are not case sensitive
# unless we tell it specifically to be case-sensitive
"TEST" -like "test"   # True
"TEST" -clike "test"  # False
```

## Querying Active Directory
Query the environments that still have an on-prem active directory setup. 

```powershell
# show the available capabilities that starts with RSAT
Get-WindowCapability -Online -Name RSAT*

# install a capability
Add-WindowsCapability -Online -Name Rsat.ActiveDirectory.DS-LDS.Tools~~~0.0.1.0

# shows prepackaged queries
Search-ADAccount -AccountDisabled
Search-ADAccount -AccountExpired
Search-ADAccount -AccountInactive -DateTime '01/01/2024'
Search-ADAccount -LockedOut
Search-ADAccount -PasswordNeverExpires
```

## Customizing AD Searches
Get-AD* commands. 
```powershell
# Return every user
Get-ADUser -Filter * 

# search in specific locations, return a small set of properties for each user
Get-ADUser -Filter * -SearchBase "OU=Marketing,DC=Adatum,DC=com"

# specify what properties to return, and format to a table
Get-ADUser -Filter * -SearchBase "OU=Marketing,DC=Adatum,DC=com" -Properties LastLogonDate,department | Format-Table name,LastLogonDate,department

# to take a peek at the list of all properties
Get-ADUser -Filter * -SearchBase "OU=Marketing,DC=Adatum,DC=com" -Properties *

# search by user name
Get-ADUser Lisa

# search by filtering
Get-ADUser -Filter {department -eq 'Marketing'}
Get-ADUser -Filter {department -eq 'Marketing' -or department -eq 'Sales'}
Get-ADUser -Filter {department -like '*i*'}

# use an ldap filter
Get-ADUser -LDAPFilter "(&(objectClass=user)(department=IT))"

```

## Administering Active Directory
```powershell
Set-ADAccountPassword Lara -NewPassword (Read-Host -AsSecureString)

# see a list of locked out users
Search-ADAccount -LockedOut
# unlock by username
Unlock-ADAccount Williams

# check a user's department property
Get-ADUser Lara -Properties Department
# change a user's department value
Set-ADUser Lara -Department 'Sales'

# below parameter doesn't exist
Get-ADUser Lara -Properties carLicense
# to add it: 
Set-AdUser Lara -Add @{carLicense='ABC123'}
# now have value for it:
Get-ADUser Lara -Properties carLicense

```

## Introduction to Azure PowerShell


































