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


## Formatting pipeline output


## Passing pipeline data by value


## Passing pipeline data by property name


## Using parentheses to change the order of operations


## Measuring objects


## Sorting objects




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
