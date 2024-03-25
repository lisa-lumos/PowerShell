# 5. PSProviders and PSDrives
## PSProviders and PSDrives
PSProviders are code that allows us to interact with various data stores in Windows, as if these data stores are a file system.

```powershell
# show which providers are installed
# and their maps to each drives
Get-PSProvider

# Get a list of drives that are currently mapped,
# and which provider they use
Get-PSDrive

# go to the registry
cd HKLM: 
# list files
dir

# list all the item commands
Get-Command *item*
```

## The FileSystem Provider
Common commands:
- cd (Set-Location)
- dir/ls (Get-Children)
- type/cat/gc (Get-Content)
- del/erase/rd/ri/rm/rmdir (Remove-Item)

```powershell
# show all the aliases for a command
Get-Alias -Definition Remove-Item

New-PSDrive -Name X -PSProvider FileSystem -Root \\Server\Share -Persist

# map a new network drive
New-PSDrive -Name X -PSProvider FileSystem -Root \\LONSVR1\C$

# navigate to this new drive called X: 
X:
# ls
dir

# Change back to C drive
c:
# navigate to the ps dir
cd \ps

# show a list of drives, and drive X will be there. 
# all powershell drives you map will not show up in File Explorer, 
# they only exist in PowerShell.
Get-PSDrive  

# remove the mapped drive
Remove-PSDrive x r

# The "Persist" flag saves the drive to user profile and File Explorer, 
# and if user log out and log in again, 
# it will still be there. 
# Explorer only takes drive name as a single letter.
New-PSDrive -Name X -PSProvider FileSystem -Root \\LONSVR1\C$ -Persist 
```

## Registry Provider
A new item will create a new key in the registry. 

```powershell
# a new item will create a new value, in that key in the registry
# this registry value governs whether remote desktop is enabled or not
# 0 means remote desktop is enabled
Get-ItemPropertyValue 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\' -Name fDenyTSConnections

# set a registry value
Set-ItemProperty 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\' -Name DENYALL -Value 3
```

## Certificate Provider
```powershell
cd cert:
dir
cd .\\LocalMachine\
dir

# show all the certificates on this machine
dir -Recurse

# show all the certificates on this machine, 
# that are going to expire in the next 60 days.
dir -Recurse -ExpiringInDays 60  

# show a list of commands for certificate management
Get-Command -Module PKI
```

## WS-MAN Provider
WS-MAN: Web Services Management Protocol.

Could modify the trusted host list, etc. 

```powershell
cd wsman:
dir
cd .\localhost\
dir
cd .\Client\
dir
# it contains TrustedHosts, which is important

cd ..
cd .\Service\
dir
```
