# 6. Variables, Arrays and Hash Tables
This is on PowerShell Scripting. Instead of one-liners, we will start to write scripts. 

## Understanding Variable Fundamentals
Variable is a named memory location. Instead of using a hex code to refer to that location, we can use a friendly name. It is a storage location where we can put a piece of text, an entire file, a collection of files, a database, etc. There are a lot of pre-defined variables that are already a part of PowerShell, but we can also create our own. The variable itself is a container of memory space. 

Be consistent in your naming and casing of your variables. Avoid space in variable name. 

To create an ad-hoc variable: 
```shell
> $computers = 'loncl1', 'londc1', 'lonsvr1'
> $computers
loncl1
londc1
lonsvr1
```

If restart PowerShell, the variable will be gone. 

A variable can also be a query result:
```shell
> $users = Get-ADUser -Filter *
> $users
(will show all user objects)
> $users.count
(will return num of users in this $users variable)
```

## Using Variables and Strings
Most of the time, PS doesn't care about whether you use single quotes or double quotes to enclose a string. But if there is var inside double quotes, PS knows it is a variable, while if the var is in side single quote, PS will treat var name as a plain string. 
```shell
> "This is me"
This is me
> 'This is me'
This is me
> $val = 'string'
> "This is a $val"
This is a string
> 'This is a $val'
This is a $val
```
If in double quotes, you want the first value to be string literal, while another value to be replaced by var value, we can use backtick sign to skip it, so the $ after it is just a plain $. backtick before an n creates a new line. backtick before a t creates a tab. backtick is also the line continuation character in PS to break up a long line. 
```shell
> $val = 'string'
> "The value of $val is $val"
The value of string is string
> "The value of `$val is $val"
The value of $val is string
```
sub expression inside double quotes, or use concatenation:
```shell
> "There are ($users.count) users in the variable"
> "There are " + $users.count + " users in the variable"
```















