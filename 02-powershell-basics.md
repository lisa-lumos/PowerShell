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


## Understanding Cmdlet Structure


## Getting Help


## Finding Commands


## Working with PowerShell Modules





































