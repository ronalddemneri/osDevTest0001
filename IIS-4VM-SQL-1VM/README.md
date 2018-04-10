# IaaS IIS and SQL on Windows Server 2016

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Ffixx220%2FARMTemplates%2Fmaster%2FIIS-4VM-SQL-1VM%2Fazuredeploy.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png" />
</a>
<a href="https://portal.azure.us/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Ffixx220%2FARMTemplates%2Fmaster%2FIIS-4VM-SQL-1VM%2Fazuredeploy.json" target="_blank">
    <img src="http://azuredeploy.net/AzureGov.png" />
</a>
<a href="http://armviz.io/#/?load=https%3A%2F%2Fraw.githubusercontent.com%2Ffixx220%2FARMTemplates%2Fmaster%2FIIS-4VM-SQL-1VM%2Fazuredeploy.json" target="_blank">
    <img src="http://armviz.io/visualizebutton.png"/>
</a>

This template creates one to four Windows Server 2016 VM(s) with IIS configured using DSC. It also installs one SQL Server 2016 SP1 standard edition VM, a VNET with two subnets, NSG, load balancer, NATing and probing rules.

<b>Context:</b><br>
I'm creating this template as a learning tool and modifying one of the quickstart templates created by GitHub User:  alibaloch

<b>You can find the original here:</b>
<a href="https://github.com/Azure/azure-quickstart-templates/tree/master/iis-2vm-sql-1vm">
https://github.com/Azure/azure-quickstart-templates/tree/master/iis-2vm-sql-1vm
</a><br>
I chose this template as a base because it's well put together, both the template itself and the GitHub integrations, images etc.

## Resources
The following resources are created by this template:
- 1 to 4 Windows Server 2016 VMs with the IIS role installed
- 1 Windows Server 2016 VM with SQL Server 2016 SP1 installed.
- SQL Server has a 50GB data disk attached.
- 1 virtual network with 2 subnets (Frotend and Backend) with NSG rules.
- 1 storage account for the VHD files.
- 1 Availability Set for IIS servers.
- 1 Load balancer with NATing rules.


<img src="https://raw.githubusercontent.com/fixx220/ARMTemplates/master/IIS-4VM-SQL-1VM/images/resources.png" />


## Architecture Diagram
<img src="https://raw.githubusercontent.com/fixx220/ARMTemplates/master/IIS-4VM-SQL-1VM/images/architecture.png" />


## Dependencies Diagram
<img src="https://raw.githubusercontent.com/fixx220/ARMTemplates/master/IIS-4VM-SQL-1VM/images/dependencies.png" />

## PowerShell Deployment

Here is some example code for deploying this template using PowerShell.  Remeber to log into your Azure account first using <b>"Login-AzureRMAccount"</b>
<br>

```PowerShell
# Deployment Variables
$DeploymentName = "Modify this per deployment e.g. Cust1001 (customerPrefixID001)" # Increment the number for subscequent deployments
$RGName = "ResourceGroupName"
$RGLocation = "southcentralus" # Change this as required
$TemplatePath = "Enter the path to your ARM template e.g. C:\ARMTemplates\azuredeploy.json"

# Create new Resource Group for Template Deployment
New-AzureRmResourceGroup -Name $RGName -Location $RGLocation

# Deploy IISandSQL-ManDisksandAS ARM Template
New-AzureRmResourceGroupDeployment -Name $DeploymentName `
    -customerPrefixID "5 character customerID e.g. Cust1" `
    -ResourceGroupName $RGName `
    -TemplateFile $TemplatePath `
    -adminUsername "AdministratorUsername" `
    -adminPassword ("AdministratorPassword" | ConvertTo-SecureString -AsPlainText -Force) `
    -dnsLabelPrefix "DNSLabelHere (Lowercase)" `
    -webServerVMSize "Standard_A1" ` # Change as required, allowed values are listed in the template under parameter of the same name
    -numberOfWebServers 2 ` # Change as required, allowed values are listed in the template under parameter of the same name
    -sqlServerVMSize "Standard_DS1" ` # Change as required, allowed values are listed in the template under parameter of the same name
    -storageAccountType "Standard_LRS" # Change as required, allowed values are listed in the template under parameter of the same name
```