# 6. Querying systems using WMI and CIM
## WMI (Windows Management Instrumentation)
Provides standards and processes of managing large enterprise environments. 

It is primarily useful as a querying tool to query/change management information from Windows systems, and make changes to these systems. Can do this using other programming languages as well. 

Common commands: `Get-WmiObject`, `Invoke-WmiMethod`, `Register-WmiEvent`. 

WMI supports event subscriptions, for example, you tell WMI to monitor your Domian Admins Global Group, and if it changes, WMI sends an email. 

```powershell
# get a list of everything, starting at the root of the namespace
Get-WMIObject -list -Namespace root -Recurse

# Can use WMI Explorer to explore modules
```

## CIM (Common Information Model) 
Your code talks to the broker (object manager), that lives in the remote machine that is being managed. 
```powershell
# equivalent to Get-WmiObject
Get-CimInstance 

Invoke-CimMethod

New\Get-CimSession

Get-WMIObject Win32_LogicalDisk -Filter "DriveType=3 AND FreeSpace>=53687091200"
```

## WQL Syntax
```
Get-WMIObject -query "Select * From Win32_LogicalDisk"
Get-WMIObject -query "Select * From Win32_LogicalDisk Where DriveType = 3"
Get-CIMInstance -query "Select * From Win32_LogicalDisk Where DriveType = 3"

Get-WMIObject -query "Select * From Win32_Service Where Name = 'audiosrv'"
Get-WMIObject -query "Select * From Win32_Service Where Name LIKE '%audio%'"
Get-WMIObject -query "Select * From Win32_Service Where NOT Name LIKE '%audio%'"
Get-WMIObject -query "Select * From Win32_Service Where Name LIKE '[af]%'"
Get-WMIObject -query "Select * From Win32_Service Where Name LIKE '[a-f]%'"
Get-WMIObject -query "Select * From Win32_Service Where Name LIKE '[^a-f]%'"
```

## Working with Registry using WMI
skip
