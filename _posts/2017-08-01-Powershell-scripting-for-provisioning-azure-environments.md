---
title: Powershell scripting for provisioning Azure environments
author: Ravin Perera
linkedin: https://www.linkedin.com/in/ravin-perera-61b4662b/
---

At Geveo, we mainly use Microsoft Azure as our cloud service provider. Every cloud software project requires the creation of several resources in Azure for hosting/functionality purposes. These resources may consist of web app service plans, databases, service bus and various other services provided by Azure. While the simplest application may only require a web app service plan and a database, medium to large-scale applications may need numerous resources such as Redis cache, multiple service bus queues, storage accounts, multiple app service plans etc... These resources needs to be created at the start of the project and needs to be altered as the project expands.

For applications with more than a handful of resources, it can be a tedious task to manually create these resources with the correct pricing plans and all the configurations. One common pitfall I've seen over and over is some resources gets created in the wrong geographical region by mistake. It can be quite time consuming to correct such a mistake as you would need to delete and recreate your resources in the correct data center.

It becomes increasingly challenging as you introduce DEV, QA, UAT and LIVE environments in to the mix. If we assume that the application contains 10 resources (which can easily be the case for most projects), it would amount to a total of 40 resources across all the environments. Further complicating things, we often need to make changes and minor additions to each environments as the project progresses. Everytime that happens, the change needs to be done separately for each environment.
It would be highly error-prone to create and maintain all these resources with correct configurations by hand. That's why you need Azure powershell scripting to help you automate environment setup.

### AzureRM powershell cmdlets

Microsoft provides a set of [powershell cmdlets] to interact with Azure Resource Manager. [Azure Resource Manager] is the service responsible for provisioning and managing resources in Azure data centers. The Azure Resource Manager powershell cmdlets have the indentifier <code>"*-AzureRM*" </code>.
It should also be noted that there's a set of legacy cmdlets which begin with <code>"*-Azure*"</code>. These employ the use of Azure classic provisioning service which was named "Azure Service Manager". There are times you have to use these in places where Microsoft still haven't ported cmdlets to the new Resource Manager model.
Tip: when you are googling for a cmdlet name which provides a specific functionality, use the "AzureRM" keyword as well (e.g. "azurerm cmdlet create sql database"). Otherwise you might get search results for the legacy cmdlets.
You can [install the entire suite of AzureRM cmdlets] by running Install-Module. You should have these installed on the machine that you intend to run your provisioning script.
<pre>
 Install-Module AzureRM
</pre>

#### Azure subscription login
You should have administrator access for an Azure subscription in order to login to the Azure portal and manage resources. This is true for AzureRm cmdlets as well. There are cmdlets for initiating an Azure login session and selecting an Azure subscription. After you login and select a subscription, all subsequent operations you perform in the same powershell console will execute in that context.

```
\#Azure Login UI will be displayed after this
Login-AzureRmAccount
\# Print the list of Azure subscriptions for the logged in user
Get-AzureRmSubscription
\# Select a subscription by name
Set-AzureRmContext –SubscriptionName “<subscription name>”

```


### Building the project environment script
By combining the aforementioned cmdlets, you can come up with a script for your project which takes care of provisioning all the required resources for one environment. You can run same script to create all the environment by following a simple parameterized naming convention.

#### Script Structure
<b>Environment name:</b> To maintain a consistent naming convention, you can use the environment name as the prefix or suffix for all the resource names. So, the environment name will be one of the parameters to the script.

<b>Data Center Location:</b> Data center geographical location will also be a parameter to the script. This is from one of the [documented strings] which identifies a specific Azure data center. All the resources will get created in the specified data center location. This is useful when you want to create your DEV resources closer to where your development center is, and LIVE resources closer to your customers' geographic region.

<b>Resource Group:</b> Ideally, your project should create a ["Resource Group"] for each environment. This makes it easy to have a separation between resources in different environments. Hence the creation of the resource group is the first operation in the script. All subsequent resources will be created using that resource group as a parameter.
To help build project-specific scripts, I’ve come up with a helper script with a set of powershell functions which follows the aforementioned parameters and naming conventions. Depending on the project, you can include the helper script in your provisioning script and call the functions in the order you want to get the resources created.

The helper script contains functions for following purposes:

* Installing AzureRM powershell module if not already installed.
* Select the Azure subscription with the specified name.
* Creating a resource group.
* Creating a web app service plan.
* Creating an application insights resource.
* Creating a web app with the specified app settings, connection strings and a custom domain.
* Creating a SQL Azure Server with firewall rules.
* Creating a SQL Database.
* Creating a storage account, blob containers and storage tables.
* Creating a Service Bus namespace and queues.
* Creating a RedisCache instance.
* It also supports a “Fake” run mode so you can test your provisioning script flow without actually creating resources in Azure.

#### Idempotent scripts
A useful quality of a provisioning scripts is for it to be idempotent, which means that the script can be run repeatedly any number of times. The script should create any resources which is not yet created and bypass the creation of the resource already exists.
To provide such a facility, you need to be able to check for existence of a resource by its name before attempting to create it. For most resources, you can use [Find-AzureRmResource] cmdlet to check for existence. For some, you have rely on other ways. If you can live with the exceptions that get thrown when you are trying to create a duplicate resource, you can keep your script simple. The powershell console simply prints the exception and continues executing the remainder of the script.

### Helper script and sample
[Here] is my helper script and an example provisioning script which makes use of the helper script. I haven’t done any checks for resource existence in order to keep things simple.


[//]: #comments 
   [powershell cmdlets]:<https://docs.microsoft.com/en-us/azure/azure-resource-manager/powershell-azure-resource-manager>
   [Azure Resource Manager]:<https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview>
   [install the entire suite of AzureRM cmdlets]:<https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps?view=azurermps-4.1.0>
   [documented strings]:<https://docs.microsoft.com/en-us/powershell/module/azurerm.resources/get-azurermlocation?view=azurermps-4.1.0>
   ["Resource Group"]:<https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview#resource-groups>
   [Find-AzureRmResource]:<https://docs.microsoft.com/en-us/azure/azure-resource-manager/powershell-azure-resource-manager#search-for-resources>
   [Here]:<https://github.com/GeveoAu/BlogRepos/tree/master/PowerShell%20Scripts>
   


