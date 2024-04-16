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

