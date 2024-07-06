# 6. Variables, Arrays and Hash Tables
This is on PowerShell Scripting. Instead of one-liners, we will start to write scripts. 

## Understanding Variable Fundamentals
Variable is a named memory location. Instead of using a hex code to refer to that location, we can use a friendly name. It is a storage location where we can put a piece of text, an entire file, a collection of files, a database, etc. There are a lot of pre-defined variables that are already a part of PowerShell, but we can also create our own. The variable itself is a container of memory space. 

Be consistent in your naming and casing of your variables. Avoid space in variable name. 

To create an ad-hoc variable: 
```powershell
> $computers = 'loncl1', 'londc1', 'lonsvr1'
> $computers
loncl1
londc1
lonsvr1
```

If restart PowerShell, the variable will be gone. 

A variable can also be a query result:
```powershell
> $users = Get-ADUser -Filter *
> $users # (will show all user objects)
> $users.count # (will return num of users in this $users variable)
```

## Using Variables and Strings
Most of the time, PS doesn't care about whether you use single quotes or double quotes to enclose a string. But if there is var inside double quotes, PS knows it is a variable, while if the var is inside single quotes, PS will treat var name as a plain string. 
```powershell
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
```powershell
> $val = 'string'

> "The value of $val is $val"
The value of string is string

> "The value of `$val is $val"
The value of $val is string
```

expression inside double quotes need to be inside parenthesis, or use concatenation:
```powershell
> "There are ($users.count) users in the variable"
> "There are " + $users.count + " users in the variable"
```

## Casting Variable Types
Declare what type of data is stored in the variable so it is represented properly. PowerShell is a strongly typed language that behaves as if it is type-less. Casting is considered as a best practice. Common types are:
- string
- int
- double
- bool
- datetime
- Type Operators (-is, -isnot, -as)

```powershell
> [string]$computerName
> $computerName = 'LON-DC1' # single quotes indicate we are passing a string
> $computerName = 1234 # because this var is a string type, so 1234 is passed as a str

> $computerName.GetType() # return the data type of this variable
string 

> $num = 1234 # assign 1234 to a var that has no casting
> $num.GetType()
Int32

> [double]$num2 = 12.34567
> $num2
12.34567

> $num2 | Get-Member
TypeName: System.Double
...

> $num2 -as [int] # use the as type operator to convert type temporarily
12

> [datetime]"1/1/2022" # it got other properties because it is now a datetime type
Saturday, January 1, 2022 12:00:00 AM

> [datetime]"1/1/2022" | Get-Member
TypeName: System.DateTime
...

> $computers = 'LONCL1' # without casting, computers var is a string only
> $computers += 'LONDC1'
> $computers
LONCL1LONDC1

> [string[]]$computers = 'LONCL1' # cast it as a string list, then can append elem to it
> $computers += 'LONDC1'
> $computers
LONCL1
LONDC1
```

## Variable properties and methods
To work with individual properties and methods inside a variable:
```powershell
> $users | Get-Member # Get-Member shows you what is in the variable
> $users. # use Tab key to loop over properties of a variable
> $users.surname # users is not an object, it is a collection of objects, but PW3 allow to fetch properties out of each object in a collection
> $nic = Get-WMIObject Win32_NetworkAdapter -Filter "PhysicalAdapter=$true"
> $nic # returns adapters
> $nic | Select * # show every property in the pipeline
```

## Strings


## Dates


## Arrays


## Array Operators


## Importing Data from Files


## .NET ArrayLists


## Hash Tables


## Custom Hash Tables





