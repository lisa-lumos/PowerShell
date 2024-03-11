# 4. PowerShell Pipeline
## Pipeline intro
The pipeline exist in any shell. 

By far, most of the commands that are talked about are "single stage pipelines", as they are single commands. But, you can often take the results of that command, and pipe it to another command, and so on. 

This is where this "lego-block mentality" came from. There are a lot of little commands that each do one thing, vs a monolithic command that do a million of things. 

```powershell
# see if both servers are online
"Server1","Server2" | Test-NetConnection

# get a list of all the VMs on a Hyper-v host, and turn on resource metering on all of them
Get-VM | Enable-VMResourceMetering

# 3 stages: 
Get-ComputerInfo | ConvertTo-Html | Out-File report.html

```

These are still "one-liners".

## Get member
PowerShell produces objects in the pipeline, as opposed to text. 

```powershell
# will return the obj type, its methods, and its properties
Get-ADUser Lara -Properties carLicense | Get-Member

Get-ADUser Lara -Properties * | Get-Member

# get a property's value of an object
(Get-ADUser Lara -Properties *).carLicense

# save this object into an variable, then access its properties
$lara = Get-ADUser Lara -Properties *
$lara.carLicense

$lara.carLicense -eq $null # will return False
$lara.Department -like 'IT*' # will return false
```

## Formatting pipeline output
Be aware that all of these formatting commands fundamentally change the nature of the object in the pipeline. So, for human to view the formatted results, always format it at the end of the pipeline. You cannot do anything to the formatted results anymore. 

The only exception is to use Out commands (to a file, to print, to null, etc) after the format commands, because the former doesn't try to interpret what is in the pipeline. 

```powershell
# Returns the result as a list by default
Get-ADUser - Filter {Department -eql 'IT'}

# Return the selected properties in a list
Get-ADUser - Filter {Department -eql 'IT'} | Format-List Name, GivenName, Surname, Enabled

# Return the selected properties in a table
Get-ADUser - Filter {Department -eql 'IT'} | Format-Table Name, GivenName, Surname, Enabled

# Format-Wide can only return one single property
Get-ADUser - Filter {Department -eql 'IT'} | Format-Wide Name

# Do not do this: 
# Return the selected properties in a table, then port them to a csv file
# It appears to return a bunch of random information
# of formatting instructions
Get-ADUser - Filter {Department -eql 'IT'} | Format-Table Name, GivenName, Surname, Enabled | Export-CSV C:\PS\users.csv

```

## Passing pipeline data by value
There are two ways to pass data down the pipeline to another command:
1. By value
2. By property name

```powershell
# it shows the ComputerName parameter accept pipeline input, 
# both ByValue and ByPropertyName
help Test-NetConnection -full

# test connection of the two computers
'LONDC1','LONSVR1' | Test-NetConnection

```

Assume "c:\ps\computers.txt" contains: 
```
LONDC1
LONSVR1
LONCLI1
```

To test connection for them:
```powershell
cd c:\ps
Get-Content .\computers.txt | Test-NetConnection

```

## Passing pipeline data by property name
Assume "c:\ps\computers.csv" contains: 
```
ComputerName
LONDC1
LONSVR1
LONCLI1
```

```powershell
# see it has a property called ComputerName
Import-CSV .\computers.csv | Get-Member

# shows this parameter accept input by ByPropertyName,
# and it accepts the parameter input as a string
help Test-NetConnection -Parameter ComputerName

# it tests the connection of each row in the csv as ComputerName value
Import-CSV .\computers.csv | Test-NetConnection

```

Assume "c:\ps\computers.csv" contains: 
```
ComputerName,Port
LONDC1,88
LONSVR1,445
LONCLI1,135
```

```powershell
# it tests the connection of each row in the csv as ComputerName value, and Port value
Import-CSV .\computers.csv | Test-NetConnection

```

If the csv col name and the property name in the command doesn't exact match, it will return an error, because the bindings doesn't work. We can rename the properties in the pipeline to fix it. 
```powershell
# shows the computers have a Name property, but no ComputerName property
Get-ADComputer -Filter *
```

## Using parentheses to change the order of operations
```powershell
# example:
(Get-NetIPAddress -InterfaceAlias Ethernet -AddressFamily IPv4).IPAddress

Test-Connection -ComputerName (Get-Content .\computers.txt)
```

## Measuring objects
```powershell
# count how many objects from the pipeline
Get-ADUser - Filter {Department -eql 'IT'} | Measure-Object

# get the number of processes
Get-Process | Measure-Object

# aggregate the numeric values of the WS property in these objects
Get-Process | Measure-Object -Property WS -Sum -Average -Maximum -Minimum

```

## Sorting objects
```powershell
Get-Process

# sort the processes by id number, in ascending order
Get-Process | Sort-Object ID

# sort the processes by id number, in descending order
Get-Process | Sort-Object ID -Descending

# sort the processes by id number, in descending order, only see unique ones
Get-Process | Sort-Object ID -Descending -Unique

# sort the processes by process name, then by ID number
Get-Process | Sort-Object ProcessName,ID

# sort the processes by process name ascending, then by ID number descending 
# (use 2 hash tables to make it work)
Get-Process | Sort-Object @{Expression='ProcessName';Descending=$false},@{Expression='ID';Descending=$true}

```

## Selecting objects



## Creating calculated properties using select-object




## filtering objects




## Enumerating objects




## Pipe output to files

Output to a text file (Easy for a person to read, but not easy to pull back into PowerShell):

`Get-ADuser -Filter * -SearchBase "OU=Marketing, DC=Adatum, DC=Com"`

`Get-ADuser -Filter * -SearchBase "OU=Marketing, DC=Adatum, DC=Com" | Format-Table`


`Get-ADuser -Filter * -SearchBase "OU=Marketing, DC=Adatum, DC=Com" | Format-Table | Out-File C:\PS\marketing.txt`

`nodepad .\marketing.txt`

If want to work with these contents later on, could output as csv, JSON, XML format:

`Get-ADuser -Filter * -SearchBase "OU=Marketing, DC=Adatum, DC=Com" | Export-CSV marketing.csv`

`nodepad .\marketing.csv`

To not have the first row that starts with # in previous csv:
`Get-ADuser -Filter * -SearchBase "OU=Marketing, DC=Adatum, DC=Com" | Export-CSV marketing.csv -NoTypeInformation`

`Get-ADuser -Filter * -SearchBase "OU=Marketing, DC=Adatum, DC=Com" |Select Name,enabled | Export-CSV marketing.csv -NoTypeInformation`

Later can import it back to PS. Problem with csv is that it is 2D, if want memberOf property (which is multi valued) in csv:

`Get-ADuser -Filter * -SearchBase "OU=Marketing, DC=Adatum, DC=Com" -Properties memberOf |Select Name,enabled,memberOf | Export-CSV marketing.csv -NoTypeInformation`

The exported file only has a placeholder for the group membership for the column. If you want to import it in the future, you have lost this membership info. So, need to use a hierarchy format like JSON or XML.

`Get-ADuser -Filter * -SearchBase "OU=Marketing, DC=Adatum, DC=Com" -Properties memberOf |Select Name,enabled,memberOf | Export-CLIXML marketing.xml`

`Get-ADuser -Filter * -SearchBase "OU=Marketing, DC=Adatum, DC=Com" -Properties memberOf |Select Name,enabled,memberOf | ConvertTo-JSON`

`Get-ADuser -Filter * -SearchBase "OU=Marketing, DC=Adatum, DC=Com" -Properties memberOf |Select Name,enabled,memberOf | ConvertTo-Html`

`Get-ADuser -Filter * -SearchBase "OU=Marketing, DC=Adatum, DC=Com" -Properties memberOf |Select Name,enabled,memberOf | ConvertTo-Html | Out-File marketing.html`
