1.

Which of the following statements accurately describes the function of Azure Private Endpoints in the context of Azure Storage?

 

Azure Private Endpoints are used to set up cross-region replication for storage accounts.

Azure Private Endpoints provide secure access by using public IP addresses to connect to Azure Storage.

Incorrect

Azure Private Endpoints enable secure connections by assigning a private IP from your VNet to the Azure Storage account.

2.

Which security feature of Azure Storage assigns a private IP address from your VNet to the storage account, ensuring network isolation?

 

Service Endpoints

Private Endpoints

Correct

NSG Rules

3.

While reviewing a configuration script, you find a line that restricts access to an Azure storage account. Which command correctly restricts access to a specific subnet?

 

az storage account update --name mystorageaccount --default-action Allow

az storage account create --name mystorageaccount --location eastus

Incorrect

az storage account network-rule add --resource-group myresourcegroup --account-name mystorageaccount --subnet /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/virtualNetworks/{vnetName}/subnets/{subnetName}

4.

Your organization has a compliance requirement to ensure data is stored in multiple regions for disaster recovery and also demands low latency within the primary region. Which Azure replication strategy fulfills these requirements?

Geo-zone-redundant storage (GZRS)

Correct

Read-access geo-redundant storage (RA-GRS)

Locally redundant storage (LRS)

5.

Which Azure Storage replication strategy provides the highest data availability and durability by synchronously replicating data across three availability zones in the same region and also copies the data to a secondary geographic region?

 

Geo-redundant storage (GRS)

Geo-zone-redundant storage (GZRS)

Correct

Locally redundant storage (LRS)

6.

After setting up a custom domain for Azure Blob Storage, access attempts fail. Which configuration should you double-check to resolve the issue?

 

Ensure the CNAME record correctly points to the storage account.

Correct

Check if the storage account is in the same region as the custom domain's registrar.

Verify that the storage account is set to a premium tier.

7.

How does Read-access geo-redundant storage (RA-GRS) improve upon standard Geo-redundant storage (GRS) in terms of data access?

 

RA-GRS allows read access to data in the secondary region, enhancing availability during primary region failures.

Correct

RA-GRS synchronously replicates data across zones within the primary region.

RA-GRS ensures data is only stored within the primary region for compliance.

8.

What is a common mistake when setting up a custom domain for Azure Blob Storage that can lead to access issues?

 

Not verifying the domain ownership before mapping it to the storage account.

Correct

Using an Azure Function to access the blob data

Configuring the storage account for Read-Access Geo-Redundant Storage (RA-GRS)

9.

Your company wants to ensure that blob data in Azure Storage can be accessed through a custom domain securely. Which protocol should you ensure is enabled to maintain secure access?

 

FTP

HTTPS

Correct

SMTP