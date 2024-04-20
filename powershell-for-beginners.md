# Powershell for Beginners

Powershell is an *Object Oriented* scripting language which is used on Windows. There are lots of different versions of powershell. In order to find out which version you are using, you can type `$host` or `$psversiontable` in your powershell session.

>[!TIP]
>When we first open powershell, it is a good idea to run `update-help` which will download the latest help files

## Aliases

In powershell, we often use shorter ways of writing commands - these are called *aliases* If we want to list *all* aliases, we can use `Get-Alias` We can use the same command with an alias to find out what it is short for: `Get-Alias cls`

## Get-Help

This is a very important cmdlet since it helps us understand cmdlets and how to use them. We can use it with the name of a cmdlet: `Get-Help Get-Service` We can also put the name of the cmdlet followed by `-?` which will give us the help for it: `Get-Service -?`

>[!NOTE]
>Powershell *cmdlets* use the same structure which is *verb-noun*

We can see *examples* of how to use cmdlets by adding the `-examples` parameter: `Get-Help Get-Service -examples`

>[!TIP]
>To see the help pages one at a time we can use `Help` for example `Help Get-Service`

As with all cmdlets, we can tab through the parameters which are available to us when using `Get-Help` We can do this by repeatedly pressing the `Tab` key after `-` like so `Get-Help Get-Service -` followed by the `Tab` key.

One useful parameter to use with the `Get-Help` cmdlet is `ShowWindow` as it opens the help in a new window.

When we look at the syntax of the `Get-Help` documentation, we can break it down as is shown below.

`Get-Service [[-Name] <String>[]] [-ComputerName <String[]>]`

Any parameter - such as `-Name` and `-ComputerName` in the above example - which has `[]` around it is *optional* - we do not have to provide it.

Any parameter - such as `-Name` in the above example - which has `[]` around the parameter name itself is positional - we do not have to name it when we use it.

>[!TIP]
>If we want to know more about the position of parameters we can use `Get-Help` with the `-Full` switch

The `<String>` part specifies the *datatype* which is expected whilst the `[]` after the datatype tells us that more than one of those datatypes can be used, for example two strings for the example of `-Name` in the above example.

There is usually more than one block of syntax given in the results of `Get-Help` - each block has slightly different parameters which can be used. Sometimes, if we include a particular parameter we will not be able to include other specific parameters - the syntax blocks show us which parameters can be used alongside each other.

>[!NOTE]
>The `Get-Command` cmdlet can also give us lots of useful data relating to commands - we can use `Get-Command -Module *Hyper*` to find cmdlets in the specified module

One way we can work with `Get-Command` and `Get-Help` is to first of all find a command using `Get-Command` and then learn more about it using `Get-Help` so we know how to use the command.

>[!TIP]
>Using the wildcard `*` lets us find data quickly without needing to remember specific cmdlet names

## Running Commands

### Integrated Scripting Environment

The ISE is a place we can write scripts in powershell. It is useful because *intellisense* is used in it so we get suggestions for what we can do via *snippets*.

>[!TIP]
>We can execute whole scripts using `F5` or just the selected line using `F8`

### Object Properties and Methods

Powershell returns *objects* when we run cmdlets. These objects have *properties*. An example would be that `ProcessName` and `Id` are two of the properties which are returned when we run the cmdlet `Get-Process`

### Parameters

We can specify parameters for cmdlets by using a single `-`

An example would be using the `-Name` parameter with the `Get-Process` cmdlet to specify the name of the process which we want to know more about: `Get-Process -Name lsass`

### Get-Member

We can find the *methods* and *properties* of an object by using `Get-Member`

`Get-Process | Get-Member`

The above command will return the properties and methods of objects returned when we run `Get-Process`

## Working With Data and Variables

### Datatypes

We can use different datatypes in powershell. If we want to know more about the methods we can invoke and the properties we can call with a specific datatype, we can use: `8 | Get-Member` - this example will list the methods for *integer* datatypes since `8` is an integer.

In order to call properties on objects we can enclose the object in `()` like so: `("hello").Length`

In order to invoke a method on an object we can do as above but include `()` with the method like so: `('hello world').Split(' ')`

We can combine commands as in this example which splits the initial string on the blank space and creates an array of two strings from the output of the `split` method before finding the `Length` property of that array: `('hello world').Split(' ').Length`

### Variables

To declare variables in powershell we use `$` before the name of the variable. We can use `=` to assign a value to a variable. An example of declaring a variable and assigning a value to it is: `$user = 'BillyBob'`

We can find the datatype of the value assigned to a variable by using: `$user.Get-Type()`

If we want to ensure that only a specific datatype is allowed to be assigned to a variable, we can use `[]` when we declare it: `[String]$city = 'Swansea'` If we do not do this, we will be able to assign different datatypes to the variable.

If we want to find out more about all of the variables which we have access to, we can go the the *variable drive* and list its contents. We can navigate to it using: `Set-Location Variable:`

We can assign objects returned from cmdlets to variables for example: `$cmdProcess = Get-Process cmd`

We can then invoke methods and access properties related to the object via the variable like so: `$cmdProcess.Kill()`

## Powershell Providers and PSDrives

Powershell providers let us access data stores in a way which is similar to navigating a file system. The interface we use is very much like the one we use to interact with a mounted drive. This means we can easily navigate different resources such as the registry or file system easily within our powershell session.

>[!TIP]
>We can see our PS providers using `Get-PSProvider`

PSDrives are the access points for providers. They are not mapped drives and they will end once we exit our powershell session.

We can list the PSDrives which are available to us using: `Get-PSDrive`

>[!NOTE]
>In order to access a *PSDrive* we need to add a `:` at the end of its name - for example `Variable:`

We can create our own PSDrives like so:

```powershell
New-PSDrive -Name dcShare -PSProvider FileSystem -Root '\\HYDRA-DC\C$\Users\tstark\Documents'
```

We can specify drives on our local machine or other machines on our domain as the `-Root` path for the PSDrive we are creating. This means we can easily navigate around a domain and or copy data across machines.

We could, for example, interact with the `tstark\Documents` directory on the `HYDRA-DC` machine in the above example by using: `Set-Locaction dcShare:`

If we want to copy data we could use: `Copy-Item -Path dcShare:\creds.txt -Destination C:\Looted`

When we are creating a new PSDrive, we can use the `-Credential` parameter like so:

```powershell
New-PSDrive -Name dcShare -PSProvider FileSystem -Root '\\HYDRA-DC\C$\Users\tstark -Credential (Get-Credential)'
```

We can remove PSDrives by using `Remove-PSDrive dcShare`

## Hash Tables

In the powershell ISE, we can create a hash table using:

```powershell
$users = @{
    'mmouse' = 'mouseymcmouseface123'
    'dduck' = 'duckymcduckduck321'
    'ysam' = 'sixgunlover888'
}
```

We can then invoke different methods on the hash table object. We can, for example, add new name:value pairs using `$users.Add('bbunny', 'ilikecarrots738')`

>[!NOTE]
>We can add name:value pairs to hash tables using `$users += @{'peye' = 'sailorman333'}`

We can remove a name:value pair using `$users.Remove('ysam')`

We can also access the keys and values of hash tables. A way to access the values is to use dot notation with the name specified for example `$users.dduck`

By default, hash tables are unordered data collections. We can get ordered - sorted - versions of them using `[ordered]` like so: `$pets = [ordered]@{ 'cat' = 'Garfield'; 'dog' = 'Lassie'; 'horse' = 'mred'}`

>[!TIP]
>We use `;` as the delimitor when we are creating hash tables in powershell on one line

## Arrays

Arrays in powershell are ordered data collecions like lists in python. We can create them just by assigning more than one value to a variable: `$cities = 'Brum', 'London', 'Manchester'`

We can combine different datatypes in arrays.

We can use indexes to access values in arrays: `$cities[0]`

We can use the shorthand `+=` to add items to arrays: `cities += 'Liverpool'`

As for hash tables, there are methods we can invoke on arrays.

## Selecting, Sorting and Finding Data

When it comes to selecting, sorting and finding data, there are three very important commands:

- `Select-Object`
- `Where-Object`
- `Sort-Object`

We can use `Sort-Object` to sort data according to different *properties* it has. An example would be sorting the contents of a directory by the length of the files in it using: `gci | Sort-Object -Property Length` We can then specify *how* we want to sort the data. An example is if we want to see only the *unique* values - we can do this using the `-Unique` parameter: `gci | Sort-Object -Property Length -Unique`

The `Select-Object` cmdlet lets us select objects based on different criteria. We can, for example, select the properties of objects to make a custom table of results: `Get-Process | Select-Object ProcessName,CPU`

>[!NOTE]
>It is common to see the alias `Select` used instead of `Select-Object`

>[!TIP]
>We can cast the output of `Select-Object` to be a *string* by using `-ExpandProperty` for example: `Get-Process | Select-Object -ExpandProperty Processname`

If we want to exclude properties from the returned data, we can use the `-ExcludeProperty` parameter.

### Where-Object

The syntax for `Where-Object` introduces a new idea which is that we can use `$_` or on powershell versions from `3.0` we can also use `$Psitem` to refer to the current object which is being piped to a new command.

An example of this would be using `Where-Object` to filter the output from `Get-Process` so that only processes which have a value of greater than `5.0` for the `CPU` property are returned. In order to understand this example, we need to be aware that when the output from `Get-Process` is piped to a new command it is done so one object at a time.

```powershell
Get-Process | Where-Object {$_.CPU -GT 5.00}
```

In this example, we are using a `-FilterScript` with the `Where-Object` cmdlet. We do not need to explicitly specify the `-FilterScript` parameter name as it is in the first position.

We then use `$_` to refer to the current object which has been passed to `Where-Object` via a pipe from `Get-Process`

We then use `.CPU` to access the `CPU` property of the current object.

The value of the `CPU` property is then compared to our specified condition of it being greater than 5.00 which we write as `-GT 5.00`

In this way, only processes which have a value of greater than 5.00 for their CPU property will be returned.

>[!NOTE]
>In versions of powershell from 3.0 we can use `$Psitem` instead of `$_` to refer to the current object

We can use `-eq` in our filterscript if we want to only return *exact* matches to our specified value or we can use `-like` if we want to use wildcards. We can also use `-ne` which means *not equal*.

>[!TIP]
>We can use aliases for `Where-Object` such as `Where` or `?`

Here are some examples which use the ideas from above.

```powershell
Get-Service | Where {$_.Name -Like '*its'}

Get-Service | Where {$Psitem.Name -eq 'bits'}

Get-Service | ? {$_.Name -ne 'bits'}
```

>[!NOTE]
>In more recent versions of powershell we do not need to use `{}` around the `-FilterScript`

### Order of Commands

When working with these three cmdlets, it is best to *select* the objects first and then *filter* and *sort* them. An example of this follows:

```powershell
Get-Service | Select Name,CanStop | Where {$_.CanStop -eq 'True'} | Sort-Object Name -Descending
```

>[!TIP]
>Since powershell returns a list when there are five or more properties to return we can force the output to be a table using the `Format-Table` cmdlet

## Doing More with Objects

### Out-File

We can use the `Out-File` cmdlet to direct the output of a command to a specified file. An example would be sending output to a .txt file so we can work with it later:

```powershell
Get-EventLog -LogName Security -Newest 10 | Select InstanceId,Message | Out-File 'C:\Users\fcastle\Documents\sec_10.txt'
```

### Get-Content

We can use `Get-Content` to bring data into powershell from external files - we usually use .txt files with `Get-Content` as there are different cmdlets to work with .csv data.

```powershell
$sec10 = Get-Content 'C:\Users\fcastle\Documents\sec_10.txt'
```

### Custom Headers using Select-Object

We can create our own headers using custom headers with the `Select-Object` cmdlet. We can specify a *name* and an *expression* using this technique.

The following example will hopefully make this more clear.

In powershell, we can use `Test-Connection` to check if hosts are live by using ICMP packets of data - this is the powershell way to do `ping`

```powershell
Test-Connection HYDRA-DC -Count 1
```

If we want to see just a `True` or `False` value we can use the `-Quiet` parameter.

We can test more than one machine by creating a variable which contains as strings the names of the machines we want to test.

```powershell
$hosts = 'HYDRA-DC', 'THEPUNISHER', 'SPIDERMAN'

$hosts | ForEach-Object { Test-Connection $_ -Count 1 -Quiet }
```

The above command uses the `ForEach-Object` cmdlet to test the connection to each object contained in the variable `$hosts` - we do not see the machine names, however, just `True` or `False`

We could solve this problem in a simple way by including the string which is being tested but it does not look elegant and is not very readable.

```powershell
$hosts | ForEach-Object { $_; Test-Connection $_ -Count 1 -Quiet }
```

To create a more readable output we can use custom headers.

```powershell
$hosts | ForEach-Object { $_ | Select @{ Name='HostName';Expression={ $_ } } }
```

This first custom header shows us a newly created header which has the name of `HostName` with the strings from the `$hosts` variable as its values.

We can add another custom header which will use the returned boolean values from the `Test-Connection` cmdlet as its values. This will create a more readable format for the output of the `Test-Connection` command which we want to run.

```powershell
$hosts | ForEach-Object { $_ | Select @{ Name='HostName';Expression={ $_ } }, @{ Name='Up';Expression={ Test-Connection $_ -Count 1 -Quiet } } }
```

