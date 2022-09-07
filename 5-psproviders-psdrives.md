# 5. PSProviders and PSDrives

## PSProviders and PSDrives
PSProviders are code that allows us to interact with various data stores in Windows as if these data stores are a file system.

`Get-PSProvider`

`cd HKLM:` - Set location - Now in the Registry.

`dir` - get child item.

`Get-Command *item*`

## FileSystem Provider

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
