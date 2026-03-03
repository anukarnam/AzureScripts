# Azure FinOps
This solution provides a simplified approach to collecting Azure budget data across all resource groups using a single API call.

## Problem Statement
Collecting Azure budget data for FinOps reporting is complicated because:
1. **Power BI with Custom Connector** - Has limitations when updating reports in Power BI Online
2. **PowerShell per-RG iteration** - Requires querying each resource group individually, adding complexity with pipeline dependencies

## Solution
The Microsoft Consumption API supports querying budgets at the **subscription level**, which returns **all budgets in a single API call** — including those scoped to individual resource groups.

### API Endpoint
```
GET /subscriptions/{subscriptionId}/providers/Microsoft.Consumption/budgets?api-version=2024-08-01
```

## How It Works
When you call the subscription-level Budget API, it returns all budgets regardless of their scope:
- Subscription-level budgets
- Resource group-scoped budgets

## Usage
### Prerequisites
- Azure PowerShell module (`Az`) installed
- Authenticated to Azure (`Connect-AzAccount`)
- Reader access to the subscription

### Running the Script
```powershell
.\GetBudgetSubLevel.ps1
```

### CSV Export
The script automatically exports data to `AzureBudgets.csv` for Power BI integration.

## Power BI Integration Options
1. **Direct REST Query** — Use Power BI's Web connector to call the API directly (if authentication allows)
2. **Scheduled Export** — Run the PowerShell script on a schedule (Azure Automation or Logic App) to export CSV to a storage account, then connect Power BI to the storage account

## Configuration
Update the `$subscriptionId` variable in the script to match your target subscription:

`powershell'
$subscriptionId = 'your-subscription-id-here'


## License
MIT
