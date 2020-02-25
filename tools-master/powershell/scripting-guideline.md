# PowerShell Scripting Guindlines

## Table of Content

- [PowerShell Scripting Guindlines](#powershell-scripting-guindlines)
  - [Table of Content](#table-of-content)
  - [Naming Guidelines](#naming-guidelines)
  - [Capitalization Rules for Identifiers](#capitalization-rules-for-identifiers)
  - [Capitalizing Compound Words and Common Terms](#capitalizing-compound-words-and-common-terms)
  - [Word Choice](#word-choice)
  - [Readability](#readability)
    - [Indent your code](#indent-your-code)
    - [Format Output](#format-output)
    - [Avoid backticks](#avoid-backticks)
    - [Avoid double quotes](#avoid-double-quotes)
    - [Line Length](#line-length)
    - [Blank lines](#blank-lines)
    - [Spaces](#spaces)
    - [Semicolons](#semicolons)
  - [Function Structure](#function-structure)
    - [Functions](#functions)
  - [Documenting and Comments](#documenting-and-comments)
  - [Keep it Simple (KISS)](#keep-it-simple-kiss)
  - [Building Reusable Tools](#building-reusable-tools)
    - [Tool or Controller Script](#tool-or-controller-script)
    - [Don't re-invent the wheel](#dont-re-invent-the-wheel)
  - [Links](#links)

## Naming Guidelines

Following a consistent set of naming conventions in the development of a script can be a major contribution to the framework’s usability. It allows the script to be used by many developers on widely separated projects. Beyond consistency of form, names of script elements must be easily understood and must convey the function of each element.

## Capitalization Rules for Identifiers

To differentiate words in an identifier, capitalize the first letter of second word in the identifier. Do not use underscores to differentiate words, or for that matter, anywhere in identifiers. There are two appropriate ways to capitalize identifiers, depending on the use of the identifier:

- camelCasing
- PascalCasing

- **DO** use *camelCasing* for all functions, variables and parameters consisting of multiple words.

## Capitalizing Compound Words and Common Terms

Most compound terms are treated as single words for purposes of capitalization.

- **DO NOT** capitalize each word in so-called closed-form compound words.

These are compound words written as a single word, such as endpoint. For the purpose of casing guidelines, treat a closed-form compound word as a single word. Use a current dictionary to determine if a compound word is written in closed form.

| Correct  | Incorrect |
| -------- | --------- |
| callback | callBack  |
| fileName | filename  |
| id       | ID        |
| signIn   | signOn    |
| signOut  | signOf    |
| userName | username  |

## Word Choice

- **DO** choose easily readable identifier names.

For example, a property named ``horizontalAlignment`` is more English-readable than ``alignmentHorizontal``.

- **DO** favor readability over brevity.

The property name ``canScrollHorizontally`` is better than ``scrollableX`` (an obscure reference to the X-axis).

- **DO** use the full name of each command.

For example, use ``Get-WmiObject -Class Win32_service``, rather than ``gwmi -Class win32_service``

- **DO** use full parameter names.

For example, use ``{{Get-WmiObject -Class win32_service -Property name, state}}``, rather than ``{{Get-WmiObject win32_service name, state}}``

- **DO NOT** use underscores, hyphens, or any other nonalphanumeric characters.
- **DO NOT** use Hungarian notation.
- **AVOID** using identifiers that conflict with keywords of widely used programming languages.
- **DO NOT** use abbreviations or contractions as part of identifier names.

For example, use ``getWindow`` rather than ``getWin``.

## Readability

### Indent your code

```powershell
if (condition1) {
    commandsToExecute
}
[elseif (condition2) {
    commandsToExecute
}]
else {
    commandsToExecute
}
foreach (item in collection) {
    scriptBlock
}
while (condition) {
    commandBlock
}
```

### Format Output

Use square brackets (`[...]`) every time you need to output variable and/or dynamic value. It helps identify changeable portion of an output:

```powershell
Write-Host "Updating Computer [$env:COMPUTERNAME] ..."
```

### Avoid backticks

Avoid using those backticks as "line continuation characters" when possible:

```powershell
Get-WmiObject -Class Win32_LogicalDisk `
              -Filter 'DriveType=3' `
              -ComputerName 'SERVER2'
```

They're hard to read, easy to miss, and easy to mistype. Also, if you add an extra whitespace after the backtick in the above example, then the command won't work. The resulting error is hard to correlate to the actual problem, making debugging the issue harder.
Here's an alternative:

```powershell
$GetWmiObjectParams = @{
    Class = 'Win32_LogicalDisk'
    Filter = 'DriveType=3'
    ComputerName = 'SERVER2'
}
Get-WmiObject @GetWmiObjectParams
```

The technique is called splatting. It lets you get the same nice, broken-out formatting without using the backtick. You can also line break after almost any comma, pipe character, or semicolon without using a backtick.

### Avoid double quotes

**DO** always make sure you prefer **single quotes** by default. Single quotes guarantee that the text will not be changed by PowerShell. What you see is what you get. It is recommended to avoid double quotes if there are no variables or escape sequences in strings.

**DO** use **double-quotes** only when you explicitly want PowerShell to make changes to your text.

There are some good reasons to use double-quotes:

- Expanding Variables

Variables remain active inside of double-quoted text and will be replaced by their content. PowerShell colorizes variables in red, and variables inside a double-quoted string remain red.

- Adding Escape Sequences

Another good reason for using double-quotes is to insert PowerShell escape sequences. Escape sequences always start with a backtick ``(`)`` and can add line feeds, tabulators, etc.

### Line Length

Limit lines to 115 characters when possible.

The PowerShell console is, by default, 120 characters wide, but it allows only 119 characters on output lines, and when entering multi-line text, PowerShell uses a line continuation prompt: ``>>>`` and thus limits your line length to 116 anyway.

The preferred way to avoid long lines is to use splatting and PowerShell's implied line continuation inside parentheses, brackets, and braces -- these should always be used in preference to the backtick for line continuation when applicable, even for strings:

```powershell
Write-Host ("This is an incredibly important, and extremely long message. " +
            "We cannot afford to leave any part of it out, nor do we want line-breaks in the output. " +
            "Using string concatenation let's us use short lines here, and still get a long line in the output")
```

### Blank lines

Single blank line may be used sparingly to separate groups of related functions, or within functions to indicate logical sections (e.g. before a block comment).

### Spaces

Lines should not have trailing whitespace. Extra spaces result in future edits where the only change is a space being added or removed, making the analysis of the changes more difficult for no reason.

You should us a single space around parameter names and operators, including comparison operators and math and assignment operators, even when the spaces are not necessary for PowerShell to correctly parse the code.
Use a single space after commas and semicolons, and around pairs of curly braces.
Avoid extra spaces inside parenthesis or square braces.

### Semicolons

Avoid using semicolons `;` at the end of each line.

PowerShell will not complain about extra semicolons, but they are unnecessary, and get in the way when code is being edited or copy-pasted. They also result in extra do-nothing edits in source control when someone finally decides to delete them.
They are also unnecessary when declaring hashtables if you are already putting each element on it's own line:

```powershell
#This is the preferred way to declare a hashtable if it must go past one line:
$Options = @{
    Margin = 2
    Padding = 2
    FontSize = 24
}
```

## Function Structure

### Functions

Avoid using the return keyword in your functions. Just place the object variable on its own.
When declaring simple functions leave a space between the function name and the parameters.

```powershell
function myFunction ($param1, $param2)
{
    ...
}
```

## Documenting and Comments

Comments that contradict the code are worse than no comments. Always make a priority of keeping the comments up-to-date when the code changes!
Comments should be in English, and should be complete sentences. If the comment is short, the period at the end can be omitted.
Remember that comments should serve to your reasoning and decision-making, not attempt to explain what a command does. With the exception of regular expressions, well-written PowerShell can be pretty self-explanatory.

```powershell
#Do not write:
#Increment margin by 2
$margin = $margin + 2

#Maybe write:
#The rendering box obscures a couple of pixels.
$margin = $margin + 2
```

Don't go overboard with comments. Unless your code is particularly obscure, don't precede each line with a comment -- doing so breaks up the code and makes it harder to read. Instead, write a single block comment.
Block comments generally apply to some or all of the code which follows them, and are indented to the same level as that code. Each line should start with a # and a single space.
If the block is particularly long (as in the case of documentation text) it is recommended to use the <# ... #> block comment syntax, but you should place the comment characters on their own lines, and indent the comment:

```powershell
#Requiring a space makes things legible and prevents confusion.
#Writing comments one-per line makes them stand out more in the console.

<#
    .Synopsis
    Really long comment blocks are tedious to keep commented in single-line mode
    Description
    Particularly when the comment must be frequently edited,
    as with the help and documentation for a function or script
#>
```

Comments on the same line as a statement can be distracting, but when they don't state the obvious, and particularly when you have several short lines of code which need explaining, they can be useful.
They should be separated from the code statement by at least two spaces, and ideally, they should line up with any other inline comments in the same block of code.

```powershell
$Options = @{
    Margin = 2          #The rendering box obscures a couple of pixels.
    Padding = 2         #We need space between the border and the text
    FontSize = 24       #Keep this above 16 so it's readable in presentations
}
```

## Keep it Simple (KISS)

The most efficient codes are not necessarily the most complex. On the contrary when the code is simple and straightforward, it is easier to read, execute, and review; it also runs more efficiently.

For example, Solution A:

```powershell
Get-Mailbox | Group-Object -Property Database | Select-Object Name, Count
```

Solution B:

```powershell
Get-MailboxDatabase | Select-Object Name, `
@{Name="Count";Expression={(Get-Mailbox -Database $_.Identity | Measure-Object `
).Count}}}
```

These two codes give same results, but Solution A came out in 1.75s, while Solution B took 3.02s and the first one is much more readable than second one.

## Building Reusable Tools

### Tool or Controller Script

There are two common types of "script" exist:

1. Some scripts contain tools, when are meant to be reusable. These are typically functions or advanced functions, and they are typically contained in a script module or in a function library of some kind. These tools are designed for a high level of re-use.

1. Some scripts are controllers, meaning they are intended to utilize one or more tools (functions, commands, etc) to automate a specific business process. A script is not intended to be reusable; it is intended to make use of reuse by leveraging functions and other commands

We are trying to produce tool scripts to have high level of their re-use in different process. Tools should accept input from parameters as much as possible

### Don't re-invent the wheel

There are a number of approaches in PowerShell that will "get the job done." In some cases, PowerShell has already the code to achieve your objectives. If that code meets your needs, then you might save yourself some time by leveraging it, instead of writing it yourself.

For example:

```powershell
function pingComputer ($computerName)
{
    $ping = Get-WmiObject Win32_PingStatus -filter "Address='$computerName'"
    if ($ping.StatusCode -eq 0) {
        return $true
    }
    else {
        return $false
    }
}
```

This function has a few opportunities for enhancement. First of all, the parameter block is declared as a basic function; using advanced functions is generally preferred. Secondly, the command verb ("Ping") isn't an approved PowerShell command verb. This will cause warnings if the function is exported as part of a PowerShell module, unless they are otherwise suppressed on import.
Thirdly, there's little reason to write this function in PowerShell v2 or later. PowerShell v2 has a built-in command that will allow you to perform a similar function. Simply use:

```powershell
Test-Connection $computerName -Quiet
```

This built-in command accomplishes the exact same task with less work on your part - e.g., you don't have to write your own function.
It has been argued by some that, "I didn't know such-and-such existed, so I wrote my own." That argument typically fails, so before making the effort to write some function or other unit of code, find out if the shell can already do that. Ask around. And, if you end up writing your own, be open to someone pointing out a built-in way to accomplish it.

## Links

- <https://github.com/PoshCode/PowerShellPracticeAndStyle>
- <https://www.simple-talk.com/wp-content/uploads/imported/2289-PSPunctuationWallChart_1_0_3.pdf>