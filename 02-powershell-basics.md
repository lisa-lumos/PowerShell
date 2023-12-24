# PowerShell basics
## Overview
It is a cmd interface. Scripting language. More like a administrative tool, not so much for developers. 

The main unit is a commandlet - a command that just performs one operation. 

Workflows provides an easy way to resume an activity, after it gets interrupted. It also provide a method of parallelization, to do tasks concurrently. 

Can execute external commands.

## Powershell Versions and Editions
Most systems have both the 32-bit and 64-bit versions of PowerShell installed. You want to use the 64-bit version exclusively, because most of the PS functionality comes from extensions, and 64-bit extensions cannot load in 32-bit PS env. 

When you launch PS, it will not automatically run a admin. If you need to run as admin, you need to click "Run as Administrator" from the start menu, or, right click the app, then run as admin. Then, you can see in the top bar, showing "Select Administrator: Windows PowerShell". 

The dollar symbol indicates that this is a variable. 
```powershell
$PSVersionTable   # automatic variable, shows version of PS running
```

The core version of the PS, will install on Linux, MacOS, etc. It will continue to be developed and maintained for years to come. 

## Command Line Fundamentals
You can configure the look and feel of PS console, through the properties of the console window. 

Also transparency, etc. 

`Start-Transcript` cmd will automatically log all commands/results, as well as context in cur session into a txt file, for your future reference. `Stop-Transcript` cmd will stop the logging; closing the console works in the same way. These commands can be added to your PS profile to execute automatically. 

## Working with Keyboard Shortcuts
up/down arrow key to scroll through history. This history is persistent, even if you close it and relaunch. 

Tab for auto-complete. Powershell is case-insensitive. But using cases can make the command easier to read. More tabs to switch between matched commands. Shift-tab to go back in direction. 

When using parameters in a command, with dash, tab can cycle through all options for parameter names. Sometimes, tab can even auto-fill parameter values. e.g.: `Get-Process -Name ApplicationFrameHost`. 

Control + Space will show you all the possible values. And you can use the arrow keys to navigate among them. 

Control + R will do a backward search through your command history. 

`Get-PSReadLineKeyHandler` command will show all the existing shortcut key mappings. 

## Introduction to Windows Terminal
The new Windows Terminal has more functionalities than the PS default terminal. You can download it from the Windows App Store. 

I allows multiple versions of PS running, with separate environments. It has different versions of shell, such as PS core, Azure Cloud Shell, Ubuntu (if you have the Windows subsystem for Linux installed), etc. 

You can also split the window into separate panes. 

## Understanding Cmdlet Structure
Some parameters are mandatory, some are optional. 
```powershell
# commandName -ParamName ParamValue ...
Get-Service
Get-Service -Name wuauserv          # show the status of a service
Get-Service -Name wuauserv,wsearch  # show the status of multiple services

# show the status of multiple services, from a remote computer
Get-Service -Name wuauserv,wsearch -ComputerName LONDC1,LONSVR1
```

Positional parameters: do not need to supply their names, as long as you supply their values in the right position. For example, the "Name" param is positional in Get-Service command, while the "ComputerName" param is not positional, you have to supply its param name to the command. 
```powershell
Get-Service wuauserv,wsearch -ComputerName LONDC1,LONSVR1
```

## Getting Help
```powershell
# show everything at once, even if content is very long
Get-Help Get-Service 

# more commonly used
# will not show everything at once, let you scroll to show more
help Get-Service
help Get-Service -Full
help Get-Service -ShowWindow  # display in GUI
help Get-Service -Examples

# use admin to update cmd help
# default help doesn't have much content,
# because most people nowadays do not actually use it. 
# but if you use it often, 
# it is best practice to run it, whenever install new modules
Update-Help

# use wildcard
help about*
```

In the syntax part of the docs, anything in the square brackets are optional. Some parameter name and value pairs are optional, some parameter name is optional, meaning it is a positional parameter. 

## Finding Commands
There are many many commands available in PowerShell, so half of the battle is finding the command for the job, rather than writing a tool from scratch. 

```powershell
# shows help for each matched command
help *user*

# only show a list of matched command names/aliases/functions/executables
Get-Command *user*

# show a list of matched ones in a particular module
Get-Command *user* -Module ActiveDirectory

# Need internet access
# Reaches out to the PowerShell library to search
# this shows everything that is tagged as NTFS
Find-Command -tag NTFS

# this pops up a GUI for you to search through the list
Show-Command
Show-Command Get-Hotfix

# to see if there are aliases defined for a command
Get-Alias -Definition my_command

# cleans the screen alias
cls
# read a file alias
cat
# ask help on a alias
help cat
# show who this alias is for
Get-Command cat
```

## Working with PowerShell Modules
Modules adds new functionality to PowerShell. They are packaged, and can be downloaded from intranet, Github, etc. 

The PowerShell gallery is the default location to search for new modules, but you can choose to register internal/external repositories that you trust. 

Modules not only have to be installed to be able to use, they also have to be loaded into the current session. Since PS3, auto-loading got introduced, so the modules will load automatically for the user. 

```powershell
# shows what is currently loaded in the the session
# not the ones that you have installed
Get-Module  # in this case, the ActiveDirectory model is not loaded

# this command is from the ActiveDirectory module
Get-ADUser Administrator

Get-Module  # now, it shows the ActiveDirectory model is loaded

# List all the installed modules in your system
Get-Module -ListAvailable

# look for azure module, from the PowerShell gallery, via internet
# returns the most recent version
Find-Module Az

# find all versions of this module
Find-Module Az -AllVersions

# install a module
Install-Module Az
```

One of the benefits of installing modules in this way, is that we can keep them up-to-date in the same way, or un-install it. Thus it supports robust package management. 

You can also browse the modules here: `powershellgallery.com`. There also displays how you can install a specific module. 
