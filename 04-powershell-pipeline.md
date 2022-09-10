# 4. PowerShell Pipeline

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
