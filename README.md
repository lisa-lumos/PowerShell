# PowerShell Notes

## Using the PowerShell Pipeline

### Pipe output to files

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

### PSProviders and PSDrives
PSProviders are code that allows us to interact with various data stores in Windows as if these data stores are a file system.

`Get-PSProvider`

`cd HKLM:` - Set location - Now in the Registry.

`dir` - get child item.

`Get-Command *item*`

### FileSystem Provider

Common commands:
- cd (Set-Location)
- dir/ls (Get-Children)
- type/cat/gc (Get-Content)
- del/erase/rd/ri/rm/rmdir (Remove-Item)

To show the aliases for a command:
`Get-Alias -Definition Remove-Item`.

Mapping a network drive:

`New-PSDrive -Name X -PSProvider FileSystem -Root \\Server\Share -Persist`

`New-PSDrive -Name X -PSProvider FileSystem -Root \\LONSVR1\C$`

`X:`

`dir`

To change back to C drive: `c:`.

`get-psdrive` will show a list of drives, and drive X will be there. But they will not show up in File Explorer - they only exist in PowerShell.

`Remove-PSDrive x` removes the drive.

`New-PSDrive -Name X -PSProvider FileSystem -Root \\LONSVR1\C$ -Persist` - The `Persist` flag saves the drive to user profile and File Explorer, and if user log out and log in again, it will still be there. Explorer only takes drive name as a single letter. 
