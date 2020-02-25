# Terraform Scripting Guindlines

## Table of Content

- [Terraform Scripting Guindlines](#terraform-scripting-guindlines)
  - [Table of Content](#table-of-content)
  - [Naming Guidelines](#naming-guidelines)
  - [Syntax](#syntax)
  - [Readability](#readability)
    - [Resources](#resources)
    - [Indent your code](#indent-your-code)
    - [Spaces](#spaces)
    - [Workspace Structure](#workspace-structure)
  - [Variables](#variables)
    - [Description](#description)
    - [Structure](#structure)
  - [Locals](#locals)
  - [Modules](#modules)

## Naming Guidelines

Following a consistent set of naming conventions in the development of a script can be a major contribution to the framework’s usability. It allows the script to be used by many developers on widely separated projects. Beyond consistency of form, names of script elements must be easily understood and must convey the function of each element.

## Syntax

The syntax of Terraform configurations is called *HashiCorp Configuration Language (HCL)*. It is meant to strike a balance between human readable and editable as well as being machine-friendly.

## Readability

### Resources

All Terraform resources should have clear name. If the name consist of two words, they should be separated by *underscore*:

```terraform
resource "azurerm_storage_account" "docs" {
    #...
}
```

- **DO NOT** use *PascalCase* or *camelCase* for any Terraform resources.
- **DO NOT** double resource type in resource's name (e.g. do not name resource `storage_account_docs` in `resource "azurerm_storage_account"` resource type)

### Indent your code

Indent your code using 4 whitespaces:

```terraform
resource "azurerm_resource_group" "example" {
    name     = "example"
    location = "eastus2"
    #...
}
```

When multiple arguments with single-line values appear on consecutive lines at the same nesting level, align their equals signs:

```terraform
client_id     = "client"
client_secret = "id"
```

For blocks that contain both arguments and "meta-arguments" (as defined by the Terraform language semantics), list meta-arguments first and separate them from other arguments with one blank line. Place meta-argument blocks last and separate them from other blocks with one blank line:

```terraform
resource "azurerm_virtual_machine" "example" {
    count = 2 #meta-argument first

    vm_size  = "Standard_DS1_v2"
    location = "eastus2"

    os_profile {
        #...
    }

    lifecycle { #meta-argument block last
        create_before_destroy = true
    }
}
```

### Spaces

Lines should not have trailing whitespace. Extra spaces result in future edits where the only change is a space being added or removed, making the analysis of the changes more difficult for no reason.

### Workspace Structure

Terraform configuration should be devided into set of `.tf` files. Each file should represent logical group of Terraform resources (e.g. `cosmos-db.tf`) and related variables (e.g. `cosmos-db-var.tf`):

```bash
├── ambra-gateway.tf
├── ambra-gateway-vars.tf
├── cosmos-db.tf
├── cosmos-db-vars.tf
├── dns.tf
|── dns-vars.tf
├── files
│   ├── helm
│   │   ├── Chart.yaml
│   │   └── templates
│   │       ├── api-gateway-cm.yaml
│   │       ├── api-gateway-dep.yaml
│   │       ├── api-gateway-svc.yaml
│   └── os
│       ├── pull-roles.yml
│       └── roles.yml
├── storage-account.tf
├── storage-account-vars.tf
└── vars-common.tf
```

## Variables

### Description

All variables should have short and clear description. The variable description should be defined within a Terraform module only.

### Structure

All variables should be stored within separate `<logical_group_name>-vars.tf` files (e.g. `cosmos-db-var.tf`). Common (shared) variables should be placed within `vars-common.tf` file.

## Locals

Local values can be helpful to avoid repeating the same values or expressions multiple times in a configuration, but if overused they can also make a configuration hard to read by future maintainers by hiding the actual values used.
Use local values only in moderation, in situations where a single value or result is used in many places and that value is likely to be changed in future. The ability to easily change the value in a central place is the key advantage of local values.

## Modules

**DO NOT** create modules that are just thin wrappers around single other resource types. If you have trouble finding a name for your module that isn't the same as the main resource type inside it, that may be a sign that your module is not creating any new abstraction and so the module is adding unnecessary complexity. Just use the resource type directly in the calling module instead.
