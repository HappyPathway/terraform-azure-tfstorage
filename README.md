# Azure Storage Account Terraform Module

A Terraform module for creating and managing Azure Storage Accounts with support for blob containers, file shares, and tables.

## Features

- Storage Account creation and management
- Multiple access tier support
- Blob container management
- File share configuration
- Table storage support
- Network rules and firewall settings
- Private endpoint integration
- Managed identity support
- Encryption configuration

## Usage

```hcl
module "storage" {
  source = "HappyPathway/tfstorage/azure"

  name                = "mystorageaccount"
  resource_group_name = "my-resource-group"
  location            = "eastus2"
  
  account_tier             = "Standard"
  account_replication_type = "LRS"
  
  containers = {
    "data" = {
      access_type = "private"
    }
    "public" = {
      access_type = "blob"
    }
  }
  
  tags = {
    Environment = "production"
    Project     = "example"
  }
}
```

## Requirements

| Name | Version |
|------|---------|
| terraform | >= 1.0.0 |
| azurerm | >= 3.0.0 |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| name | Storage account name | `string` | n/a | yes |
| resource_group_name | Resource group name | `string` | n/a | yes |
| location | Azure location | `string` | n/a | yes |
| account_tier | Storage account tier | `string` | `"Standard"` | no |
| account_replication_type | Replication type | `string` | `"LRS"` | no |
| containers | Map of container configurations | `map(object)` | `{}` | no |
| enable_https_traffic_only | Enforce HTTPS | `bool` | `true` | no |
| tags | Resource tags | `map(string)` | `{}` | no |

## Outputs

| Name | Description |
|------|-------------|
| id | Storage account ID |
| name | Storage account name |
| primary_access_key | Primary access key |
| primary_connection_string | Primary connection string |
| primary_blob_endpoint | Primary blob endpoint |

## Examples

### Basic Storage Account

```hcl
module "basic_storage" {
  source = "HappyPathway/tfstorage/azure"

  name                = "basicstore"
  resource_group_name = "my-rg"
  location            = "eastus2"
}
```

### Production Storage Account

```hcl
module "prod_storage" {
  source = "HappyPathway/tfstorage/azure"

  name                = "prodstore"
  resource_group_name = "prod-rg"
  location            = "eastus2"
  
  account_tier             = "Standard"
  account_replication_type = "ZRS"
  
  containers = {
    "data" = {
      access_type = "private"
    }
    "backup" = {
      access_type = "private"
    }
    "public" = {
      access_type = "blob"
    }
  }
  
  network_rules = {
    default_action = "Deny"
    ip_rules       = ["203.0.113.0/24"]
    bypass         = ["AzureServices"]
  }
  
  tags = {
    Environment = "production"
    Terraform   = "true"
  }
}
```

## Testing

```bash
# Initialize the module
terraform init

# Run tests
terraform test
```

## Contributing

Contributions are welcome! Please read our [Contributing Guide](CONTRIBUTING.md) for details.

## License

MIT License - see [LICENSE](LICENSE) for details