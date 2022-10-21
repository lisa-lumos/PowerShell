# 6. Querying systems using WMI and CIM

## WMI (Windows Management Instrumentation)
It is primarily useful as a querying tool  to query management information from Windows systems, and make changes to these systems. `Get-WmiObject`, `Invoke-WmiMethod`, `Register-WmiEvent`. 

## CIM (Common Information Model) 
- `Get-CimInstance` is equivalent to `Get-WmiObject`,
- `Invoke-CimMethod`
- `New\Get-CimSession`

```
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











