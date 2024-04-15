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