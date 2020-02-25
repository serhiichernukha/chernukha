# Visual Studio Code Configuration

## Required Extentions

At least the following VS Code extentions should be installed:

- Ansible
- Code Spell Checker
- JSON Tools
- MarkDown All in One
- markdownlint
- PowerShell
- Project Manager
- Terraform
- TODO Highlight

## Workspace Configuration

### Editor

Add the following lines to enable whitespace trimming:

```json
{
    "files.trimTrailingWhitespace": true
}
```

### Extensions Configuration

#### TODO Highlight

Add the following lines to highlight important keywords:

```json
{
    "todohighlight.keywords": [
        "NOTE:"
    ]
}
```