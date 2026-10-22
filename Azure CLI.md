

## 🧭 1. Syntax Patterns (learn these first)

*CLI:* az vm create -g rg1 -n vm1 
*Powershell:* New-AzVM -ResourceGroupNuma rg1 -Name vm1


### 🔐 Sign-in, context, setup

```bash
az login                                  # browser
az login --use-device-code                # device code
az account list -o table
az account set --subscription "<name-or-id>"
az account show
az configure --defaults group=rg1 location=westeurope   # stop typing -g / -l
az extension add --name <ext>             # e.g. containerapp, storagesync, dataprotection
az upgrade
```

```powershell
Install-Module Az -Scope CurrentUser
Update-Module Az
Connect-AzAccount                         # -UseDeviceAuthentication  -TenantId
Get-AzSubscription
Set-AzContext -Subscription "<name-or-id>"
Get-AzContext
```

### 🐚 Bash / output essentials

```bash
RG=rg1; LOC=westeurope                                    # variables (no spaces around =)
VMID=$(az vm show -g $RG -n vm1 --query id -o tsv)        # command substitution
az vm list -o table                                       # -o json|jsonc|table|tsv|yaml|none
az vm list --query "[].{Name:name, RG:resourceGroup}" -o table   # JMESPath
az vm list -d --query "[?powerState=='VM running'].name" -o tsv  # -d = show-details (power state, IPs)
for i in 1 2 3; do az vm create -g $RG -n vm$i --image Ubuntu2204 --generate-ssh-keys --no-wait; done
```

```powershell
Get-AzVM | Select-Object Name, Location | Format-Table
Get-AzVM | Where-Object { $_.Location -eq "westeurope" }
Get-AzVM -ResourceGroupName rg1 | Stop-AzVM -Force          # pipeline
$p = @{ ResourceGroupName="rg1"; Name="vm1"; Location="westeurope" }; New-AzVM @p   # splatting
```

> [!note] ☁️ Cloud Shell Bash **or** PowerShell, both have `az` + `Az` preinstalled · auto-authenticated · persistent storage = an **Azure Files share** (`clouddrive`) **or** ephemeral session (no storage) · ~20 min idle timeout.

---

## 🪪 2. Identities & Governance (20–25%)

### 👤 Entra ID users & groups

> [!warning] Modules **AzureAD** and **MSOnline** PowerShell modules are **retired** → use **Microsoft Graph PowerShell** (`*-Mg*`) or `Az` (`*-AzAD*`). If an answer uses `New-AzureADUser`/`New-MsolUser`, treat it as legacy.

```bash
az ad user create --display-name "Ann Lee" --user-principal-name ann@contoso.com --password '<pwd>'
az ad user list --query "[].userPrincipalName" -o tsv
az ad group create --display-name "IT-Admins" --mail-nickname itadmins
az ad group member add --group "IT-Admins" --member-id <userObjectId>
az ad sp create-for-rbac --name app1 --role Contributor --scopes /subscriptions/<sub>/resourceGroups/rg1
```

```powershell
# Az module
New-AzADUser -DisplayName "Ann Lee" -UserPrincipalName ann@contoso.com -Password $secPwd -MailNickname ann
New-AzADGroup -DisplayName "IT-Admins" -MailNickname itadmins
Add-AzADGroupMember -TargetGroupDisplayName "IT-Admins" -MemberUserPrincipalName ann@contoso.com

# Microsoft Graph PowerShell
Connect-MgGraph -Scopes "User.ReadWrite.All","Group.ReadWrite.All"
New-MgUser -DisplayName "Ann Lee" -UserPrincipalName ann@contoso.com -MailNickname ann -AccountEnabled -PasswordProfile @{ Password="<pwd>"; ForceChangePasswordNextSignIn=$true }
Update-MgUser -UserId ann@contoso.com -UsageLocation IE        # REQUIRED before assigning a licence
Set-MgUserLicense -UserId ann@contoso.com -AddLicenses @{SkuId="<skuId>"} -RemoveLicenses @()
New-MgGroup -DisplayName "Sales-Dyn" -MailEnabled:$false -SecurityEnabled -MailNickname salesdyn -GroupTypes "DynamicMembership" -MembershipRule '(user.department -eq "Sales")' -MembershipRuleProcessingState On
New-MgGroupMember -GroupId <groupId> -DirectoryObjectId <userId>
New-MgInvitation -InvitedUserEmailAddress guest@fabrikam.com -InviteRedirectUrl "https://myapps.microsoft.com" -SendInvitationMessage   # B2B guest
```

**Dynamic membership rule syntax** (Entra ID P1 required):

|Rule|Meaning|
|---|---|
|`(user.department -eq "Sales")`|equals|
|`(user.country -ne "IE")`|not equals|
|`(user.jobTitle -startsWith "Eng")` / `-contains` / `-match`|string ops|
|`(user.department -in ["Sales","HR"])`|in list|
|`(user.accountEnabled -eq true) -and (user.userType -eq "Guest")`|`-and` / `-or` / `-not`|
|`(device.deviceOSType -eq "Windows")`|device groups|
|`user.objectId -ne null`|all users|

> [!tip] Licensing trap Licence assignment **fails without `UsageLocation`**. Group-based licensing = assign licence to a group (P1).

### 🛡️ RBAC

```bash
az role assignment create --assignee ann@contoso.com --role "Virtual Machine Contributor" --scope /subscriptions/<sub>/resourceGroups/rg1
az role assignment list --assignee ann@contoso.com --all -o table
az role assignment delete --assignee ann@contoso.com --role "Reader" --resource-group rg1
az role definition list --name "Contributor"
az role definition list --custom-role-only true -o table
az role definition create --role-definition @customrole.json
az role definition update --role-definition @customrole.json
```

```powershell
New-AzRoleAssignment -SignInName ann@contoso.com -RoleDefinitionName "Reader" -ResourceGroupName rg1
New-AzRoleAssignment -ObjectId <groupOrSpId> -RoleDefinitionName "Contributor" -Scope "/subscriptions/<sub>"
Get-AzRoleAssignment -SignInName ann@contoso.com
Remove-AzRoleAssignment -SignInName ann@contoso.com -RoleDefinitionName "Reader" -ResourceGroupName rg1
Get-AzRoleDefinition -Name "Virtual Machine Contributor" | ConvertTo-Json | Out-File role.json   # start a custom role
New-AzRoleDefinition -InputFile role.json
Set-AzRoleDefinition -InputFile role.json
```

**Scope string format:**

```
/providers/Microsoft.Management/managementGroups/<mg>
/subscriptions/<subId>
/subscriptions/<subId>/resourceGroups/<rg>
/subscriptions/<subId>/resourceGroups/<rg>/providers/Microsoft.Compute/virtualMachines/<vm>
```

**Custom role JSON keywords:**

|Key|Purpose|
|---|---|
|`Actions`|allowed **control-plane** ops (`Microsoft.Compute/virtualMachines/start/action`)|
|`NotActions`|subtracted from `Actions` (**not** a deny)|
|`DataActions` / `NotDataActions`|**data-plane** (e.g. read blob contents)|
|`AssignableScopes`|where the role can be assigned (MG / sub / RG)|
|`*` wildcard|`Microsoft.Storage/*` = everything in that provider|
|`*/read`|read everything|

> [!tip] Owner vs Contributor vs User Access Administrator **Contributor** = manage everything **except** access (can't assign roles). **User Access Administrator** = only access. **Owner** = both. Role-Based Access Control Administrator = assign roles only (with conditions).

### 📜 Azure Policy

```bash
az policy definition list --query "[?displayName=='Allowed locations']" -o table
az policy definition create --name tag-env --rules @rules.json --params @params.json --mode Indexed
az policy set-definition create --name init1 --definitions @defs.json        # initiative
az policy assignment create --name allowed-locs --policy "<defNameOrId>" --scope /subscriptions/<sub>/resourceGroups/rg1 --params '{"listOfAllowedLocations":{"value":["westeurope","northeurope"]}}'
az policy assignment create ... --mi-system-assigned --location westeurope   # needed for deployIfNotExists / modify
az policy state trigger-scan --resource-group rg1                           # force compliance eval
az policy state summarize --resource-group rg1
az policy state list --filter "complianceState eq 'NonCompliant'"
az policy remediation create --name fix1 --policy-assignment allowed-locs --resource-group rg1
az policy exemption create --name ex1 --policy-assignment <id> --exemption-category Waiver
```

```powershell
$def = Get-AzPolicyDefinition | Where-Object { $_.Properties.DisplayName -eq "Allowed locations" }
New-AzPolicyAssignment -Name allowed-locs -PolicyDefinition $def -Scope "/subscriptions/<sub>" -PolicyParameterObject @{ listOfAllowedLocations = @("westeurope") }
New-AzPolicyAssignment ... -IdentityType SystemAssigned -Location westeurope
Start-AzPolicyComplianceScan -ResourceGroupName rg1
Get-AzPolicyState -ResourceGroupName rg1 -Filter "ComplianceState eq 'NonCompliant'"
Start-AzPolicyRemediation -Name fix1 -PolicyAssignmentId <id>
```

**Policy effects:** `Deny` · `Audit` · `AuditIfNotExists` · `DeployIfNotExists` · `Modify` · `Append` · `Disabled` · `DenyAction` · `Manual`

> [!tip] Remediation Only **DeployIfNotExists** and **Modify** can remediate **existing** resources → need a **managed identity** on the assignment + a **remediation task**. New assignments evaluate in ~30 min; use `trigger-scan` to force.

### 🔒 Resource locks

```bash
az lock create --name nodelete --lock-type CanNotDelete --resource-group rg1
az lock create --name ro --lock-type ReadOnly -g rg1 --resource-name acct1 --resource-type Microsoft.Storage/storageAccounts
az lock list -g rg1 -o table
az lock delete --name nodelete -g rg1
```

```powershell
New-AzResourceLock -LockName nodelete -LockLevel CanNotDelete -ResourceGroupName rg1
New-AzResourceLock -LockName ro -LockLevel ReadOnly -ResourceName acct1 -ResourceType Microsoft.Storage/storageAccounts -ResourceGroupName rg1
Get-AzResourceLock -ResourceGroupName rg1
Remove-AzResourceLock -LockName nodelete -ResourceGroupName rg1
```

> [!warning] Lock traps Locks are **inherited** downward; **most restrictive wins**; apply even to **Owners**. `ReadOnly` on a storage account blocks `listKeys` (a POST) → key-based access in portal fails. `ReadOnly` on a VM blocks **start/stop**. Only **Owner / User Access Administrator** can create/delete locks.

### 🏷️ Tags

```bash
az group update -n rg1 --tags env=prod dept=it               # REPLACES all tags
az tag update --resource-id $ID --operation Merge --tags owner=dev   # Merge | Replace | Delete
az resource list --tag env=prod -o table
az tag list
```

```powershell
New-AzTag -ResourceId $id -Tag @{ env="prod" }                 # replaces
Update-AzTag -ResourceId $id -Tag @{ owner="dev" } -Operation Merge
Get-AzResource -Tag @{ env="prod" }
Set-AzResourceGroup -Name rg1 -Tag @{ env="prod" }
```

> [!tip] Tags are **NOT inherited** from RG/subscription → use Policy "**Inherit a tag from the resource group**" (Modify effect) or "**Require a tag**" (Deny).

### 🗂️ Subscriptions, management groups, moving, cost

```bash
az account management-group create --name mg-prod --display-name "Production" --parent mg-root
az account management-group subscription add --name mg-prod --subscription <subId>
az group create -n rg1 -l westeurope
az group delete -n rg1 --yes --no-wait
az resource move --destination-group rg2 --ids <id1> <id2>          # add --destination-subscription-id for cross-sub
az consumption budget create --budget-name b1 --amount 100 --category cost --time-grain Monthly --start-date 2026-11-01 --end-date 2027-10-31
az advisor recommendation list -o table
```

```powershell
New-AzManagementGroup -GroupName mg-prod -DisplayName "Production" -ParentId "/providers/Microsoft.Management/managementGroups/mg-root"
New-AzManagementGroupSubscription -GroupName mg-prod -SubscriptionId <subId>
New-AzResourceGroup -Name rg1 -Location westeurope
Remove-AzResourceGroup -Name rg1 -Force
Move-AzResource -DestinationResourceGroupName rg2 -ResourceId $id1,$id2
New-AzConsumptionBudget -Name b1 -Amount 100 -Category Cost -TimeGrain Monthly -StartDate 2026-11-01
```

> [!note] Moving resources Resource **keeps its region** (RG location is just metadata). Source + destination RGs are **locked for writes** during the move. Moving to another **region** = **Azure Resource Mover** (not `az resource move`).

---

## 💾 3. Storage (15–20%)

### 🏦 Storage accounts

```bash
az storage account create -n acct1 -g rg1 -l westeurope --sku Standard_GRS --kind StorageV2 --access-tier Hot --min-tls-version TLS1_2 --allow-blob-public-access false --https-only true
az storage account update -n acct1 -g rg1 --sku Standard_RAGRS          # change redundancy
az storage account update -n acct1 -g rg1 --access-tier Cool
az storage account keys list -n acct1 -g rg1
az storage account keys renew -n acct1 -g rg1 --key key1                 # rotate
az storage account show-connection-string -n acct1 -g rg1
az storage account failover -n acct1 -g rg1                             # customer-managed failover (GRS/RA-GRS)
```

```powershell
New-AzStorageAccount -ResourceGroupName rg1 -Name acct1 -Location westeurope -SkuName Standard_GRS -Kind StorageV2 -AccessTier Hot -MinimumTlsVersion TLS1_2
Set-AzStorageAccount -ResourceGroupName rg1 -Name acct1 -SkuName Standard_RAGRS
Get-AzStorageAccountKey -ResourceGroupName rg1 -Name acct1
New-AzStorageAccountKey -ResourceGroupName rg1 -Name acct1 -KeyName key1    # REGENERATES key (not "new key")
Invoke-AzStorageAccountFailover -ResourceGroupName rg1 -Name acct1
```

**SKU names:** `Standard_LRS` `Standard_ZRS` `Standard_GRS` `Standard_RAGRS` `Standard_GZRS` `Standard_RAGZRS` `Premium_LRS` `Premium_ZRS` **Kinds:** `StorageV2` (default) · `BlockBlobStorage` (premium blobs) · `FileStorage` (premium files) · `Storage`/`BlobStorage` (legacy)

> [!warning] Redundancy changes LRS ↔ GRS/RA-GRS = simple `update`. **To/from ZRS/GZRS** = **conversion** (portal/support) or manual migration. Premium accounts = **LRS/ZRS only**.

### 📦 Blobs & containers

```bash
az storage container create --account-name acct1 -n c1 --auth-mode login --public-access off
az storage blob upload --account-name acct1 -c c1 -n file.txt -f ./file.txt --auth-mode login --tier Cool
az storage blob upload-batch --account-name acct1 -d c1 -s ./folder --auth-mode login
az storage blob list --account-name acct1 -c c1 --auth-mode login -o table
az storage blob download --account-name acct1 -c c1 -n file.txt -f ./out.txt --auth-mode login
az storage blob set-tier --account-name acct1 -c c1 -n file.txt --tier Archive --auth-mode login
az storage blob set-tier ... --tier Hot --rehydrate-priority High        # rehydrate from Archive
az storage blob copy start --destination-container c2 --destination-blob f.txt --source-uri <url> --account-name acct2
```

```powershell
$ctx = New-AzStorageContext -StorageAccountName acct1 -UseConnectedAccount     # Entra auth (or -StorageAccountKey)
New-AzStorageContainer -Name c1 -Context $ctx -Permission Off
Set-AzStorageBlobContent -File .\file.txt -Container c1 -Blob file.txt -Context $ctx -StandardBlobTier Cool
Get-AzStorageBlob -Container c1 -Context $ctx
Get-AzStorageBlobContent -Container c1 -Blob file.txt -Destination .\ -Context $ctx
$blob = Get-AzStorageBlob -Container c1 -Blob file.txt -Context $ctx; $blob.BlobClient.SetAccessTier("Archive")
Start-AzStorageBlobCopy -SrcContainer c1 -SrcBlob f.txt -DestContainer c2 -Context $ctx -DestContext $ctx2
```

> [!tip] `--auth-mode login` = use **Entra ID** (needs a data role e.g. **Storage Blob Data Contributor** — Owner/Contributor alone is **not** enough for data). Omit it → falls back to **account key**.

### 🔑 SAS & stored access policies

```bash
az storage container generate-sas --account-name acct1 -n c1 --permissions rl --expiry 2026-12-31T23:59Z --auth-mode login --as-user   # user-delegation SAS
az storage blob generate-sas --account-name acct1 -c c1 -n f.txt --permissions r --expiry 2026-12-31T23:59Z --https-only --account-key <key>
az storage account generate-sas --account-name acct1 --services b --resource-types sco --permissions rwdl --expiry 2026-12-31T23:59Z
az storage container policy create --account-name acct1 -c c1 -n pol1 --permissions rl --expiry 2026-12-31T23:59Z
az storage container generate-sas --account-name acct1 -n c1 --policy-name pol1          # SAS tied to policy (revocable)
```

```powershell
New-AzStorageContainerSASToken -Name c1 -Permission rl -ExpiryTime (Get-Date).AddDays(7) -Context $ctx
New-AzStorageBlobSASToken -Container c1 -Blob f.txt -Permission r -ExpiryTime (Get-Date).AddHours(4) -Context $ctx
New-AzStorageAccountSASToken -Service Blob,File -ResourceType Service,Container,Object -Permission rwdl -ExpiryTime (Get-Date).AddDays(1) -Context $ctx
New-AzStorageContainerStoredAccessPolicy -Container c1 -Policy pol1 -Permission rl -ExpiryTime (Get-Date).AddDays(30) -Context $ctx
```

|SAS type|Signed by|Revoke by|
|---|---|---|
|**User delegation**|Entra ID creds (`--as-user`)|revoke user delegation keys|
|**Service**|account key|rotate key **or** delete stored access policy (if used)|
|**Account**|account key|rotate key|

**SAS permission letters:** `r` read · `a` add · `c` create · `w` write · `d` delete · `l` list · `t` tag · `x` delete version · `f` filter **Account SAS `--resource-types`:** `s` service · `c` container · `o` object

### 🔥 Network access

```bash
az storage account update -n acct1 -g rg1 --default-action Deny --bypass AzureServices
az storage account network-rule add -n acct1 -g rg1 --ip-address 203.0.113.10
az storage account network-rule add -n acct1 -g rg1 --vnet-name vnet1 --subnet data    # needs service endpoint on subnet
az storage account update -n acct1 -g rg1 --public-network-access Disabled           # private endpoint only
```

```powershell
Update-AzStorageAccountNetworkRuleSet -ResourceGroupName rg1 -Name acct1 -DefaultAction Deny -Bypass AzureServices
Add-AzStorageAccountNetworkRule -ResourceGroupName rg1 -Name acct1 -IPAddressOrRange "203.0.113.10"
Add-AzStorageAccountNetworkRule -ResourceGroupName rg1 -Name acct1 -VirtualNetworkResourceId $subnet.Id
```

### ♻️ Data protection & lifecycle

```bash
az storage account blob-service-properties update -n acct1 -g rg1 --enable-delete-retention true --delete-retention-days 14 --enable-container-delete-retention true --container-delete-retention-days 7 --enable-versioning true --enable-change-feed true
az storage account blob-service-properties update -n acct1 -g rg1 --enable-restore-policy true --restore-days 6   # point-in-time restore (< delete-retention days)
az storage blob undelete --account-name acct1 -c c1 -n f.txt
az storage account management-policy create --account-name acct1 -g rg1 --policy @policy.json
az storage container immutability-policy create --account-name acct1 -c c1 --period 365       # time-based retention (WORM)
az storage container legal-hold set --account-name acct1 -c c1 --tags case123
```

```powershell
Enable-AzStorageBlobDeleteRetentionPolicy -ResourceGroupName rg1 -StorageAccountName acct1 -RetentionDays 14
Update-AzStorageBlobServiceProperty -ResourceGroupName rg1 -StorageAccountName acct1 -IsVersioningEnabled $true -EnableChangeFeed $true
Enable-AzStorageContainerDeleteRetentionPolicy -ResourceGroupName rg1 -StorageAccountName acct1 -RetentionDays 7
Set-AzStorageAccountManagementPolicy -ResourceGroupName rg1 -StorageAccountName acct1 -Policy $policy
```

**Lifecycle policy JSON keywords:**

```json
{ "rules": [{ "name": "tiering", "enabled": true, "type": "Lifecycle",
  "definition": {
    "filters": { "blobTypes": ["blockBlob"], "prefixMatch": ["logs/"] },
    "actions": { "baseBlob": {
        "tierToCool":    { "daysAfterModificationGreaterThan": 30 },
        "tierToArchive": { "daysAfterModificationGreaterThan": 90 },
        "delete":        { "daysAfterModificationGreaterThan": 365 } },
      "snapshot": { "delete": { "daysAfterCreationGreaterThan": 90 } } } } }] }
```

Also: `daysAfterLastAccessTimeGreaterThan` (needs **last access tracking** enabled), `daysAfterCreationGreaterThan`, `version`, `tierToCold`, `enableAutoTierToHotFromCool`.

> [!note] Object replication requires **versioning on both** accounts + **change feed on source**.

### 🚚 AzCopy

```bash
azcopy login                                                    # Entra ID (or append ?<SAS> to URL)
azcopy make  "https://acct1.blob.core.windows.net/newc?<SAS>"
azcopy copy  "./local/*" "https://acct1.blob.core.windows.net/c1?<SAS>" --recursive
azcopy copy  "https://acct1.blob.core.windows.net/c1?<SAS>" "https://acct2.blob.core.windows.net/c1?<SAS>" --recursive   # server-to-server
azcopy copy  "./data" "https://acct1.file.core.windows.net/share1?<SAS>" --recursive    # Azure Files
azcopy sync  "./local" "https://acct1.blob.core.windows.net/c1?<SAS>" --delete-destination=true
azcopy list  "https://acct1.blob.core.windows.net/c1?<SAS>"
azcopy remove "https://acct1.blob.core.windows.net/c1/old?<SAS>" --recursive
azcopy jobs list | azcopy jobs resume <jobId>
```

|Tool|Use|
|---|---|
|**AzCopy**|CLI, scripts, large/bulk, sync|
|**Storage Explorer**|GUI (uses AzCopy under the hood)|
|**Import/Export**|ship **your own disks** to Azure DC|
|**Data Box**|Microsoft-supplied appliance, TB–PB offline|

### 📁 Azure Files & File Sync

```bash
az storage share-rm create -g rg1 --storage-account acct1 -n share1 --quota 1024 --access-tier TransactionOptimized   # Hot | Cool | TransactionOptimized | Premium
az storage share-rm update -g rg1 --storage-account acct1 -n share1 --quota 2048
az storage share snapshot --account-name acct1 -n share1
az storage account file-service-properties update -g rg1 -n acct1 --enable-delete-retention true --delete-retention-days 14
az storage account update -g rg1 -n acct1 --enable-large-file-share
```

```powershell
New-AzRmStorageShare -ResourceGroupName rg1 -StorageAccountName acct1 -Name share1 -QuotaGiB 1024 -AccessTier Hot
New-AzRmStorageShare ... -Snapshot                              # (or) $share.Snapshot()
Update-AzStorageFileServiceProperty -ResourceGroupName rg1 -StorageAccountName acct1 -EnableShareDeleteRetentionPolicy $true -ShareRetentionDays 14
```

**Mounting:**

```powershell
Test-NetConnection -ComputerName acct1.file.core.windows.net -Port 445      # first check: ISPs often block 445
net use Z: \\acct1.file.core.windows.net\share1 /user:Azure\acct1 <storageKey>
```

```bash
sudo mount -t cifs //acct1.file.core.windows.net/share1 /mnt/share1 -o vers=3.1.1,username=acct1,password=<key>,serverino   # SMB
sudo mount -t nfs acct1.file.core.windows.net:/acct1/share1 /mnt/share1 -o vers=4,minorversion=1,sec=sys                     # NFS (Premium FileStorage)
```

> [!warning] SMB vs NFS **SMB** = TCP **445**, Windows/Linux/macOS, identity auth (AD DS / Entra Kerberos / Entra DS) or key. **NFS 4.1** = TCP **2049**, **Premium (FileStorage) only**, Linux, **no** key/identity auth → network-level security only (private endpoint / service endpoint), secure transfer must be **disabled**.

**File Sync order (PowerShell):**

```powershell
New-AzStorageSyncService -ResourceGroupName rg1 -Name sss1 -Location westeurope           # 1. Storage Sync Service
Register-AzStorageSyncServer -ResourceGroupName rg1 -StorageSyncServiceName sss1         # 2. on server (after installing agent)
New-AzStorageSyncGroup -ResourceGroupName rg1 -StorageSyncServiceName sss1 -Name sg1     # 3. sync group
New-AzStorageSyncCloudEndpoint -ResourceGroupName rg1 -StorageSyncServiceName sss1 -SyncGroupName sg1 -Name ce1 -StorageAccountResourceId $acct.Id -AzureFileShareName share1   # 4. ONE cloud endpoint
New-AzStorageSyncServerEndpoint -ResourceGroupName rg1 -StorageSyncServiceName sss1 -SyncGroupName sg1 -Name se1 -ServerResourceId $server.ResourceId -ServerLocalPath "D:\Data" -CloudTiering -VolumeFreeSpacePercent 20   # 5. server endpoint(s)
```

> [!tip] 1 sync group = **1 cloud endpoint** + many server endpoints. A server registers to **one** Storage Sync Service. Cloud tiering **not** on system volume (C:).

---

## 🖥️ 4. Compute (20–25%)

### 📐 ARM templates & Bicep

```bash
az deployment group create -g rg1 --template-file main.bicep --parameters @main.parameters.json env=prod
az deployment group create -g rg1 --template-file main.bicep --parameters main.bicepparam
az deployment group create -g rg1 --template-file main.json --mode Complete     # default = Incremental
az deployment group what-if -g rg1 --template-file main.bicep                    # preview changes
az deployment group validate -g rg1 --template-file main.bicep
az deployment sub create -l westeurope --template-file sub.bicep                 # subscription scope (e.g. create RGs, policy)
az deployment mg create --management-group-id mg1 -l westeurope --template-file mg.bicep
az deployment tenant create -l westeurope --template-file t.bicep
az group export -n rg1 > exported.json                                           # export RG as ARM JSON
az bicep build --file main.bicep          # Bicep → ARM JSON
az bicep decompile --file main.json       # ARM JSON → Bicep
az bicep install / az bicep upgrade
```

```powershell
New-AzResourceGroupDeployment -ResourceGroupName rg1 -TemplateFile main.bicep -TemplateParameterFile main.parameters.json -Mode Incremental
New-AzResourceGroupDeployment ... -WhatIf
Test-AzResourceGroupDeployment -ResourceGroupName rg1 -TemplateFile main.json
New-AzSubscriptionDeployment -Location westeurope -TemplateFile sub.bicep       # alias: New-AzDeployment
New-AzManagementGroupDeployment -ManagementGroupId mg1 -Location westeurope -TemplateFile mg.bicep
Export-AzResourceGroup -ResourceGroupName rg1
Get-AzResourceGroupDeployment -ResourceGroupName rg1
```

> [!warning] Deployment mode **Incremental** (default) = add/update, leave other resources alone. **Complete** = **deletes** RG resources **not in the template**. Complete mode is RG-scope only.

**ARM JSON keywords:** `$schema` · `contentVersion` · `parameters` · `variables` · `functions` · `resources` · `outputs` · `dependsOn` · `copy` (loops) · `condition` **ARM functions:** `[parameters('x')]` `[variables('x')]` `[resourceGroup().location]` `[resourceId(...)]` `[concat()]` `[uniqueString(resourceGroup().id)]` `[reference()]`

**Bicep keywords:**

```bicep
targetScope = 'resourceGroup'            // subscription | managementGroup | tenant
@allowed(['Standard_LRS','Standard_GRS'])
param skuName string = 'Standard_LRS'
@secure()
param adminPassword string
param location string = resourceGroup().location
var accName = 'st${uniqueString(resourceGroup().id)}'
resource sa 'Microsoft.Storage/storageAccounts@2023-05-01' = {
  name: accName
  location: location
  sku: { name: skuName }
  kind: 'StorageV2'
}
resource vnet 'Microsoft.Network/virtualNetworks@2023-09-01' existing = { name: 'vnet1' }   // reference existing
module net './network.bicep' = { name: 'netDeploy', params: { location: location } }
output saId string = sa.id
```

Also: `for` loops (`[for i in range(0,3): {...}]`), `if` conditions, implicit `dependsOn` via symbolic names.

### 🖥️ Virtual machines

```bash
az vm image list -o table                        # popular aliases (Ubuntu2204, Win2022Datacenter, …)
az vm image list --publisher MicrosoftWindowsServer --all -o table
az vm list-sizes -l westeurope -o table
az vm list-skus -l westeurope --zone -o table     # sizes + zone availability/restrictions
az vm create -g rg1 -n vm1 --image Ubuntu2204 --size Standard_B2s --admin-username azureuser --generate-ssh-keys --vnet-name vnet1 --subnet web --public-ip-sku Standard --zone 1
az vm create -g rg1 -n win1 --image Win2022Datacenter --admin-username azadmin --admin-password '<pwd>' --availability-set as1
az vm open-port -g rg1 -n vm1 --port 80 --priority 900          # adds NSG rule
az vm list-ip-addresses -g rg1 -n vm1 -o table
az vm get-instance-view -g rg1 -n vm1 --query instanceView.statuses[1]   # power state
az vm start | stop | deallocate | restart | delete -g rg1 -n vm1
az vm redeploy -g rg1 -n vm1                     # move to new host (fix connectivity/host issues)
az vm reapply -g rg1 -n vm1                      # reapply state (failed state)
az vm list-vm-resize-options -g rg1 -n vm1 -o table
az vm resize -g rg1 -n vm1 --size Standard_D4s_v5
az vm user update -g rg1 -n vm1 -u azureuser -p '<newpwd>'      # reset password (VMAccess)
az vm user reset-ssh -g rg1 -n vm1
az vm run-command invoke -g rg1 -n vm1 --command-id RunShellScript --scripts "sudo apt-get update"   # Windows: RunPowerShellScript
az vm extension set -g rg1 --vm-name vm1 --name CustomScript --publisher Microsoft.Azure.Extensions --settings '{"fileUris":["https://…/s.sh"],"commandToExecute":"./s.sh"}'
az vm extension list -g rg1 --vm-name vm1 -o table
az vm generalize -g rg1 -n vm1 && az image create -g rg1 -n img1 --source vm1
az vm boot-diagnostics enable -g rg1 -n vm1
az vm boot-diagnostics get-boot-log -g rg1 -n vm1
```

```powershell
New-AzVM -ResourceGroupName rg1 -Name vm1 -Location westeurope -Image Win2022Datacenter -Size Standard_B2s -Credential (Get-Credential) -VirtualNetworkName vnet1 -SubnetName web -PublicIpAddressName vm1-pip -OpenPorts 3389 -Zone 1
Get-AzVM -ResourceGroupName rg1 -Name vm1 -Status
Get-AzVMImagePublisher -Location westeurope | Get-AzVMImageOffer | Get-AzVMImageSku
Get-AzVMSize -Location westeurope
Get-AzVMSize -ResourceGroupName rg1 -VMName vm1          # sizes available for RESIZE on current cluster
Start-AzVM / Restart-AzVM / Remove-AzVM -ResourceGroupName rg1 -Name vm1
Stop-AzVM -ResourceGroupName rg1 -Name vm1 -Force         # DEALLOCATES by default
Stop-AzVM -ResourceGroupName rg1 -Name vm1 -StayProvisioned   # stopped but still billed
Set-AzVM -ResourceGroupName rg1 -Name vm1 -Redeploy
Set-AzVM -ResourceGroupName rg1 -Name vm1 -Generalized
$vm = Get-AzVM -ResourceGroupName rg1 -Name vm1
$vm.HardwareProfile.VmSize = "Standard_D4s_v5"
Update-AzVM -VM $vm -ResourceGroupName rg1                 # apply resize
Invoke-AzVMRunCommand -ResourceGroupName rg1 -VMName vm1 -CommandId RunPowerShellScript -ScriptPath .\fix.ps1
Set-AzVMCustomScriptExtension -ResourceGroupName rg1 -VMName vm1 -Location westeurope -Name cse -FileUri "https://…/s.ps1" -Run "s.ps1"
Set-AzVMExtension -ResourceGroupName rg1 -VMName vm1 -Name AMA -Publisher Microsoft.Azure.Monitor -ExtensionType AzureMonitorWindowsAgent -TypeHandlerVersion 1.0
Set-AzVMAccessExtension -ResourceGroupName rg1 -VMName vm1 -Name VMAccess -Credential (Get-Credential)   # reset admin pwd
```

**Full PS config-object build** (recognise the chain):

```powershell
$vmCfg = New-AzVMConfig -VMName vm1 -VMSize Standard_B2s -Zone 1
$vmCfg = Set-AzVMOperatingSystem -VM $vmCfg -Windows -ComputerName vm1 -Credential $cred
$vmCfg = Set-AzVMSourceImage -VM $vmCfg -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2022-datacenter -Version latest
$vmCfg = Add-AzVMNetworkInterface -VM $vmCfg -Id $nic.Id
$vmCfg = Set-AzVMOSDisk -VM $vmCfg -CreateOption FromImage -StorageAccountType Premium_LRS
New-AzVM -ResourceGroupName rg1 -Location westeurope -VM $vmCfg
```

> [!warning] Stop vs deallocate (classic trap)
> 
> |Command|State|Compute billed?|
> |---|---|---|
> |`az vm stop`|Stopped|**Yes**|
> |`az vm deallocate`|Stopped (deallocated)|**No** (disks still billed)|
> |`Stop-AzVM`|Deallocated|**No**|
> |`Stop-AzVM -StayProvisioned`|Stopped|**Yes**|
> |Shutdown from inside OS|Stopped|**Yes**|

### 💽 Managed disks

```bash
az vm disk attach -g rg1 --vm-name vm1 --name data1 --new --size-gb 128 --sku Premium_LRS --lun 0
az vm disk detach -g rg1 --vm-name vm1 --name data1
az disk create -g rg1 -n data2 --size-gb 256 --sku StandardSSD_LRS --zone 1
az disk update -g rg1 -n data1 --size-gb 512               # expand only (never shrink)
az disk update -g rg1 -n data1 --sku Premium_LRS           # change type → VM must be DEALLOCATED
az snapshot create -g rg1 -n snap1 --source data1 --incremental true
az disk create -g rg1 -n restored --source snap1            # disk from snapshot
az disk grant-access -g rg1 -n data1 --duration-in-seconds 3600   # SAS to export VHD
az vm encryption enable -g rg1 -n vm1 --disk-encryption-keyvault kv1 --volume-type All     # ADE (BitLocker/DM-Crypt)
az vm update -g rg1 -n vm1 --set securityProfile.encryptionAtHost=true                    # encryption at host
az disk-encryption-set create -g rg1 -n des1 --key-url <kvKeyUrl> --source-vault kv1      # SSE with CMK
```

```powershell
$vm = Get-AzVM -ResourceGroupName rg1 -Name vm1
$vm = Add-AzVMDataDisk -VM $vm -Name data1 -CreateOption Empty -DiskSizeInGB 128 -Lun 0 -StorageAccountType Premium_LRS
Update-AzVM -VM $vm -ResourceGroupName rg1
Remove-AzVMDataDisk -VM $vm -Name data1; Update-AzVM -VM $vm -ResourceGroupName rg1
$cfg = New-AzDiskConfig -Location westeurope -CreateOption Empty -DiskSizeGB 256 -SkuName StandardSSD_LRS; New-AzDisk -ResourceGroupName rg1 -DiskName data2 -Disk $cfg
New-AzDiskUpdateConfig -DiskSizeGB 512 | Update-AzDisk -ResourceGroupName rg1 -DiskName data1
$snap = New-AzSnapshotConfig -SourceUri $disk.Id -Location westeurope -CreateOption Copy -Incremental; New-AzSnapshot -ResourceGroupName rg1 -SnapshotName snap1 -Snapshot $snap
Set-AzVMDiskEncryptionExtension -ResourceGroupName rg1 -VMName vm1 -DiskEncryptionKeyVaultUrl $kv.VaultUri -DiskEncryptionKeyVaultId $kv.ResourceId -VolumeType All
```

**Disk SKUs:** `Standard_LRS` (HDD) · `StandardSSD_LRS` · `StandardSSD_ZRS` · `Premium_LRS` · `Premium_ZRS` · `PremiumV2_LRS` · `UltraSSD_LRS` **Inside the VM after attaching:**

```bash
lsblk                                                    # find new disk (e.g. /dev/sdc)
sudo parted /dev/sdc --script mklabel gpt mkpart xfspart xfs 0% 100%
sudo mkfs.xfs /dev/sdc1 && sudo mkdir /datadrive && sudo mount /dev/sdc1 /datadrive
sudo blkid   # → add UUID to /etc/fstab with nofail
```

```powershell
Get-Disk | Where-Object PartitionStyle -eq 'RAW' | Initialize-Disk -PartitionStyle GPT -PassThru | New-Partition -AssignDriveLetter -UseMaximumSize | Format-Volume -FileSystem NTFS
```

### 🧱 Availability sets & scale sets

```bash
az vm availability-set create -g rg1 -n as1 --platform-fault-domain-count 2 --platform-update-domain-count 5
az vmss create -g rg1 -n vmss1 --image Ubuntu2204 --instance-count 2 --vm-sku Standard_B2s --admin-username azureuser --generate-ssh-keys --orchestration-mode Uniform --upgrade-policy-mode Automatic --zones 1 2 3
az vmss scale -g rg1 -n vmss1 --new-capacity 5
az vmss update-instances -g rg1 -n vmss1 --instance-ids "*"           # Manual upgrade policy → push model update
az vmss list-instances -g rg1 -n vmss1 -o table
az monitor autoscale create -g rg1 --resource vmss1 --resource-type Microsoft.Compute/virtualMachineScaleSets --name as-vmss1 --min-count 2 --max-count 10 --count 2
az monitor autoscale rule create -g rg1 --autoscale-name as-vmss1 --condition "Percentage CPU > 70 avg 5m" --scale out 2
az monitor autoscale rule create -g rg1 --autoscale-name as-vmss1 --condition "Percentage CPU < 30 avg 5m" --scale in 1
```

```powershell
New-AzAvailabilitySet -ResourceGroupName rg1 -Name as1 -Location westeurope -PlatformFaultDomainCount 2 -PlatformUpdateDomainCount 5 -Sku Aligned   # Aligned = managed disks
New-AzVmss -ResourceGroupName rg1 -VMScaleSetName vmss1 -Location westeurope -ImageName Ubuntu2204 -InstanceCount 2 -Credential $cred -UpgradePolicyMode Automatic
$vmss = Get-AzVmss -ResourceGroupName rg1 -VMScaleSetName vmss1; $vmss.Sku.Capacity = 5; Update-AzVmss -ResourceGroupName rg1 -VMScaleSetName vmss1 -VirtualMachineScaleSet $vmss
```

> [!tip] Availability options **Availability set** = FD + UD inside **one datacenter** (99.95%). **Availability zones** = separate DCs in a region (99.99%). A VM **can't be added to an availability set after creation** → recreate. Upgrade policies: `Automatic` · `Rolling` · `Manual`. Orchestration: **Flexible** (default for new) · **Uniform**.

### 🖼️ Azure Compute Gallery

```bash
az sig create -g rg1 --gallery-name gal1
az sig image-definition create -g rg1 --gallery-name gal1 --gallery-image-definition def1 --publisher Contoso --offer Web --sku 1 --os-type Linux --os-state Generalized
az sig image-version create -g rg1 --gallery-name gal1 --gallery-image-definition def1 --gallery-image-version 1.0.0 --virtual-machine $VMID --target-regions westeurope northeurope
```

### 📦 Containers — ACR, ACI, Container Apps

```bash
az acr create -g rg1 -n contosoacr --sku Basic             # Basic | Standard | Premium
az acr login -n contosoacr
az acr build --registry contosoacr --image web:v1 .        # build in cloud (ACR Tasks) — no local Docker needed
az acr import -n contosoacr --source docker.io/library/nginx:latest --image nginx:latest
az acr repository list -n contosoacr -o table
az acr update -n contosoacr --admin-enabled true
az acr replication create -r contosoacr -l northeurope      # Premium only

az container create -g rg1 -n ci1 --image mcr.microsoft.com/azuredocs/aci-helloworld --cpu 1 --memory 1.5 --ports 80 --ip-address Public --dns-name-label ci1-demo --os-type Linux --restart-policy OnFailure --environment-variables KEY=val --secure-environment-variables SECRET=x
az container create ... --azure-file-volume-account-name acct1 --azure-file-volume-account-key <key> --azure-file-volume-share-name share1 --azure-file-volume-mount-path /mnt/data   # persistent storage
az container create ... --registry-login-server contosoacr.azurecr.io --registry-username <u> --registry-password <p>
az container logs -g rg1 -n ci1
az container attach -g rg1 -n ci1
az container exec -g rg1 -n ci1 --exec-command "/bin/sh"
az container show -g rg1 -n ci1 --query instanceView.state

az containerapp env create -g rg1 -n env1 -l westeurope
az containerapp create -g rg1 -n app1 --environment env1 --image contosoacr.azurecr.io/web:v1 --target-port 80 --ingress external --min-replicas 0 --max-replicas 10 --registry-server contosoacr.azurecr.io
az containerapp up -n app1 -g rg1 --source .               # build + deploy in one go
az containerapp update -g rg1 -n app1 --image contosoacr.azurecr.io/web:v2
az containerapp revision list -g rg1 -n app1 -o table
```

```powershell
New-AzContainerRegistry -ResourceGroupName rg1 -Name contosoacr -Sku Basic
Connect-AzContainerRegistry -Name contosoacr
$c = New-AzContainerInstanceObject -Name web -Image mcr.microsoft.com/azuredocs/aci-helloworld -RequestCpu 1 -RequestMemoryInGb 1.5 -Port @(New-AzContainerInstancePortObject -Port 80)
New-AzContainerGroup -ResourceGroupName rg1 -Name ci1 -Location westeurope -Container $c -OsType Linux -RestartPolicy OnFailure -IpAddressType Public -IPAddressDnsNameLabel ci1-demo
Get-AzContainerInstanceLog -ResourceGroupName rg1 -ContainerGroupName ci1 -ContainerName web
```

**Docker basics you may see:**

```bash
docker build -t contosoacr.azurecr.io/web:v1 .
docker push contosoacr.azurecr.io/web:v1
docker pull contosoacr.azurecr.io/web:v1
```

||ACI|Container Apps|
|---|---|---|
|Use|single containers / simple groups, batch|microservices, HTTP apps, event-driven|
|Scale|manual (no autoscale)|**KEDA** autoscale incl. **scale to zero**|
|Restart policy|`Always` (default) · `OnFailure` · `Never`|n/a (revisions)|
|Ingress/revisions|public IP + DNS label|built-in ingress, **revisions**, traffic split|

### 🌐 App Service

```bash
az appservice plan create -g rg1 -n plan1 --sku S1 --is-linux
az appservice plan update -g rg1 -n plan1 --sku P1V3 --number-of-workers 3      # scale up + out (manual)
az webapp list-runtimes --os linux
az webapp create -g rg1 -p plan1 -n contoso-web --runtime "NODE:20-lts"
az webapp up -n contoso-web --runtime "PYTHON:3.12" --sku B1                     # quick deploy from local folder
az webapp deploy -g rg1 -n contoso-web --src-path app.zip --type zip
az webapp deployment source config -g rg1 -n contoso-web --repo-url <git> --branch main
az webapp config appsettings set -g rg1 -n contoso-web --settings ENV=prod
az webapp config appsettings set -g rg1 -n contoso-web --slot-settings DB_CONN=x   # sticky to slot (doesn't swap)
az webapp config connection-string set -g rg1 -n contoso-web -t SQLAzure --settings Db='<conn>'
az webapp deployment slot create -g rg1 -n contoso-web --slot staging
az webapp deployment slot swap -g rg1 -n contoso-web --slot staging --target-slot production
az webapp traffic-routing set -g rg1 -n contoso-web --distribution staging=20      # testing in production
az webapp config hostname add -g rg1 --webapp-name contoso-web --hostname www.contoso.com
az webapp config ssl bind -g rg1 -n contoso-web --certificate-thumbprint <tp> --ssl-type SNI
az webapp update -g rg1 -n contoso-web --https-only true
az webapp vnet-integration add -g rg1 -n contoso-web --vnet vnet1 --subnet appsubnet
az webapp config backup create -g rg1 --webapp-name contoso-web --container-url "<SAS-url>" --backup-name b1
az webapp log config -g rg1 -n contoso-web --application-logging filesystem --web-server-logging filesystem
az webapp log tail -g rg1 -n contoso-web
```

```powershell
New-AzAppServicePlan -ResourceGroupName rg1 -Name plan1 -Location westeurope -Tier Standard -NumberofWorkers 2 -WorkerSize Small -Linux
Set-AzAppServicePlan -ResourceGroupName rg1 -Name plan1 -Tier PremiumV3 -NumberofWorkers 3
New-AzWebApp -ResourceGroupName rg1 -Name contoso-web -Location westeurope -AppServicePlan plan1
New-AzWebAppSlot -ResourceGroupName rg1 -Name contoso-web -Slot staging
Switch-AzWebAppSlot -ResourceGroupName rg1 -Name contoso-web -SourceSlotName staging -DestinationSlotName production
Set-AzWebApp -ResourceGroupName rg1 -Name contoso-web -AppSettings @{ ENV="prod" } -HttpsOnly $true
```

> [!tip] Plan tiers that unlock features **Free/Shared** = no custom SSL, no scale-out · **Basic** = custom domain + SSL, manual scale · **Standard+** = **deployment slots**, **autoscale**, **VNet integration** · **Premium** = more slots/instances · **Isolated** = App Service Environment (dedicated VNet). **Scale up** = change tier/size · **Scale out** = more instances (all apps in the plan scale together).

---

## 🌐 5. Virtual Networking (15–20%)

### 🕸️ VNets & subnets

```bash
az network vnet create -g rg1 -n vnet1 -l westeurope --address-prefixes 10.0.0.0/16 --subnet-name web --subnet-prefixes 10.0.1.0/24
az network vnet subnet create -g rg1 --vnet-name vnet1 -n app --address-prefixes 10.0.2.0/24
az network vnet update -g rg1 -n vnet1 --address-prefixes 10.0.0.0/16 10.1.0.0/16      # add address space
az network vnet update -g rg1 -n vnet1 --dns-servers 10.0.0.4 10.0.0.5                 # custom DNS (restart VMs to pick up)
az network vnet subnet update -g rg1 --vnet-name vnet1 -n app --delegations Microsoft.Web/serverFarms
az network vnet check-ip-address -g rg1 -n vnet1 --ip-address 10.0.1.10
```

```powershell
$web = New-AzVirtualNetworkSubnetConfig -Name web -AddressPrefix 10.0.1.0/24
$vnet = New-AzVirtualNetwork -Name vnet1 -ResourceGroupName rg1 -Location westeurope -AddressPrefix 10.0.0.0/16 -Subnet $web
$vnet = Get-AzVirtualNetwork -Name vnet1 -ResourceGroupName rg1
Add-AzVirtualNetworkSubnetConfig -Name app -VirtualNetwork $vnet -AddressPrefix 10.0.2.0/24
$vnet | Set-AzVirtualNetwork                                       # ← REQUIRED to commit
```

### 🔗 Peering

```bash
az network vnet peering create -g rg1 -n hub-to-spoke --vnet-name hub --remote-vnet spoke --allow-vnet-access --allow-forwarded-traffic --allow-gateway-transit
az network vnet peering create -g rg1 -n spoke-to-hub --vnet-name spoke --remote-vnet hub --allow-vnet-access --allow-forwarded-traffic --use-remote-gateways
az network vnet peering list -g rg1 --vnet-name hub -o table           # peeringState should be Connected
az network vnet peering sync -g rg1 -n hub-to-spoke --vnet-name hub   # after changing address space
```

```powershell
Add-AzVirtualNetworkPeering -Name hub-to-spoke -VirtualNetwork $hub -RemoteVirtualNetworkId $spoke.Id -AllowForwardedTraffic -AllowGatewayTransit
Add-AzVirtualNetworkPeering -Name spoke-to-hub -VirtualNetwork $spoke -RemoteVirtualNetworkId $hub.Id -AllowForwardedTraffic -UseRemoteGateways
Get-AzVirtualNetworkPeering -VirtualNetworkName hub -ResourceGroupName rg1
```

> [!warning] Peering traps Must be created **in both directions** (state `Initiated` until the second side exists). **Not transitive** (A↔B, B↔C ≠ A↔C → use hub NVA/firewall + UDRs, or Virtual WAN). Address spaces **must not overlap**. `--allow-gateway-transit` on the **hub** side, `--use-remote-gateways` on the **spoke** side.

### 🧱 NSGs & ASGs

```bash
az network nsg create -g rg1 -n nsg-web
az network nsg rule create -g rg1 --nsg-name nsg-web -n allow-https --priority 100 --direction Inbound --access Allow --protocol Tcp --source-address-prefixes Internet --destination-port-ranges 443
az network nsg rule create -g rg1 --nsg-name nsg-web -n deny-rdp --priority 200 --access Deny --protocol Tcp --destination-port-ranges 3389 --source-address-prefixes '*'
az network vnet subnet update -g rg1 --vnet-name vnet1 -n web --network-security-group nsg-web    # subnet association
az network nic update -g rg1 -n vm1-nic --network-security-group nsg-web                          # NIC association
az network asg create -g rg1 -n asg-web
az network nic ip-config update -g rg1 --nic-name vm1-nic -n ipconfig1 --application-security-groups asg-web
az network nsg rule create -g rg1 --nsg-name nsg-web -n web-to-db --priority 300 --source-asgs asg-web --destination-asgs asg-db --destination-port-ranges 1433 --protocol Tcp --access Allow
az network nic list-effective-nsg -g rg1 -n vm1-nic          # effective security rules
```

```powershell
$r = New-AzNetworkSecurityRuleConfig -Name allow-https -Protocol Tcp -Direction Inbound -Priority 100 -SourceAddressPrefix Internet -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 443 -Access Allow
$nsg = New-AzNetworkSecurityGroup -Name nsg-web -ResourceGroupName rg1 -Location westeurope -SecurityRules $r
Add-AzNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg -Name deny-rdp ... ; $nsg | Set-AzNetworkSecurityGroup
Set-AzVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name web -AddressPrefix 10.0.1.0/24 -NetworkSecurityGroup $nsg; $vnet | Set-AzVirtualNetwork
New-AzApplicationSecurityGroup -ResourceGroupName rg1 -Name asg-web -Location westeurope
Get-AzEffectiveNetworkSecurityGroup -NetworkInterfaceName vm1-nic -ResourceGroupName rg1
```

**Service tags** (use in source/destination): `Internet` · `VirtualNetwork` · `AzureLoadBalancer` · `Storage` / `Storage.WestEurope` · `Sql` · `AzureCloud` · `AzureMonitor` · `AppService`

> [!tip] NSG evaluation Priority **100–4096**, **lowest number wins**, first match stops processing. Inbound: **subnet NSG → NIC NSG**. Outbound: **NIC NSG → subnet NSG**. Traffic must be allowed by **both**. Default rules (65000–65500): AllowVNetInBound, AllowAzureLoadBalancerInBound, DenyAllInBound / AllowVnetOutBound, AllowInternetOutBound, DenyAllOutBound.

### 🛣️ Route tables (UDR)

```bash
az network route-table create -g rg1 -n rt-spoke --disable-bgp-route-propagation true
az network route-table route create -g rg1 --route-table-name rt-spoke -n to-fw --address-prefix 0.0.0.0/0 --next-hop-type VirtualAppliance --next-hop-ip-address 10.0.0.4
az network vnet subnet update -g rg1 --vnet-name vnet1 -n app --route-table rt-spoke
az network nic show-effective-route-table -g rg1 -n vm1-nic -o table
az network nic update -g rg1 -n nva-nic --ip-forwarding true      # required on the NVA NIC
```

```powershell
$rt = New-AzRouteTable -ResourceGroupName rg1 -Name rt-spoke -Location westeurope -DisableBgpRoutePropagation
Add-AzRouteConfig -RouteTable $rt -Name to-fw -AddressPrefix 0.0.0.0/0 -NextHopType VirtualAppliance -NextHopIpAddress 10.0.0.4 | Set-AzRouteTable
Set-AzVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name app -AddressPrefix 10.0.2.0/24 -RouteTable $rt; $vnet | Set-AzVirtualNetwork
Get-AzEffectiveRouteTable -NetworkInterfaceName vm1-nic -ResourceGroupName rg1
```

**`--next-hop-type`:** `VirtualAppliance` (needs IP) · `VirtualNetworkGateway` · `VnetLocal` · `Internet` · `None` (drop)

> [!note] Route selection: **longest prefix match** first; ties → **UDR > BGP > system**. NVA needs **IP forwarding enabled on its NIC** (Azure) **and** in the guest OS.

### 🌍 IP addressing

```bash
az network public-ip create -g rg1 -n pip1 --sku Standard --allocation-method Static --zone 1 2 3 --dns-name contoso-app
az network nic ip-config update -g rg1 --nic-name vm1-nic -n ipconfig1 --private-ip-address 10.0.1.10      # static private IP
az network nic ip-config update -g rg1 --nic-name vm1-nic -n ipconfig1 --public-ip-address pip1            # associate public IP
az network nic ip-config create -g rg1 --nic-name vm1-nic -n ipconfig2 --private-ip-address 10.0.1.11     # secondary IP
az network nat gateway create -g rg1 -n nat1 --public-ip-addresses pip-nat
az network vnet subnet update -g rg1 --vnet-name vnet1 -n app --nat-gateway nat1
```

```powershell
New-AzPublicIpAddress -ResourceGroupName rg1 -Name pip1 -Location westeurope -Sku Standard -AllocationMethod Static -Zone 1,2,3
$nic = Get-AzNetworkInterface -Name vm1-nic -ResourceGroupName rg1
$nic.IpConfigurations[0].PrivateIpAllocationMethod = "Static"; $nic.IpConfigurations[0].PrivateIpAddress = "10.0.1.10"
Set-AzNetworkInterface -NetworkInterface $nic
```

> [!tip] **Standard** public IP = **static only**, zone-redundant capable, **closed by default** (needs NSG to allow inbound). **Basic SKU public IP is retired** — if asked to pick, choose Standard. Set static private IPs **in Azure** (NIC), never inside the guest OS.

### 🧭 DNS

```bash
az network dns zone create -g rg1 -n contoso.com
az network dns zone show -g rg1 -n contoso.com --query nameServers     # → set NS records at your registrar
az network dns record-set a add-record -g rg1 -z contoso.com -n www -a 20.1.2.3
az network dns record-set cname set-record -g rg1 -z contoso.com -n app -c contoso-web.azurewebsites.net
az network dns record-set txt add-record -g rg1 -z contoso.com -n asuid.www -v <verificationId>
az network dns record-set ns add-record -g rg1 -z contoso.com -n sub -d ns1.other.com   # child zone delegation
az network private-dns zone create -g rg1 -n corp.internal
az network private-dns link vnet create -g rg1 -z corp.internal -n link-vnet1 -v vnet1 -e true    # -e = auto-registration
az network private-dns record-set a add-record -g rg1 -z corp.internal -n db -a 10.0.2.5
```

```powershell
New-AzDnsZone -Name contoso.com -ResourceGroupName rg1
New-AzDnsRecordSet -Name www -RecordType A -ZoneName contoso.com -ResourceGroupName rg1 -Ttl 3600 -DnsRecords (New-AzDnsRecordConfig -IPv4Address 20.1.2.3)
New-AzPrivateDnsZone -Name corp.internal -ResourceGroupName rg1
New-AzPrivateDnsVirtualNetworkLink -ZoneName corp.internal -ResourceGroupName rg1 -Name link-vnet1 -VirtualNetworkId $vnet.Id -EnableRegistration
```

**Testing name resolution:**

```bash
nslookup www.contoso.com ns1-01.azure-dns.com      # query Azure name server directly (before delegation)
dig www.contoso.com
```

```powershell
Resolve-DnsName www.contoso.com -Server ns1-01.azure-dns.com
```

> [!tip] Private DNS: a VNet can have **auto-registration** link to **only one** private zone, but resolution links to many. Root/apex can't be a CNAME → use **alias record**. Azure-provided DNS = **168.63.129.16**.

### 🔐 Service endpoints, private endpoints, Bastion

```bash
az network vnet subnet update -g rg1 --vnet-name vnet1 -n data --service-endpoints Microsoft.Storage Microsoft.Sql
az network service-endpoint policy create -g rg1 -n sep1                    # restrict to specific storage accounts

az network private-endpoint create -g rg1 -n pe-acct1 --vnet-name vnet1 --subnet data --private-connection-resource-id $(az storage account show -n acct1 -g rg1 --query id -o tsv) --group-id blob --connection-name pe-acct1-conn
az network private-dns zone create -g rg1 -n privatelink.blob.core.windows.net
az network private-dns link vnet create -g rg1 -z privatelink.blob.core.windows.net -n link1 -v vnet1 -e false
az network private-endpoint dns-zone-group create -g rg1 --endpoint-name pe-acct1 -n default --private-dns-zone privatelink.blob.core.windows.net --zone-name blob

az network vnet subnet create -g rg1 --vnet-name vnet1 -n AzureBastionSubnet --address-prefixes 10.0.250.0/26
az network public-ip create -g rg1 -n pip-bastion --sku Standard
az network bastion create -g rg1 -n bas1 --vnet-name vnet1 --public-ip-address pip-bastion --sku Standard --enable-tunneling true
az network bastion ssh -g rg1 -n bas1 --target-resource-id $VMID --auth-type ssh-key --username azureuser --ssh-key ~/.ssh/id_rsa
az network bastion rdp -g rg1 -n bas1 --target-resource-id $VMID      # native client (Windows), Standard+
```

```powershell
New-AzPrivateEndpoint -ResourceGroupName rg1 -Name pe-acct1 -Location westeurope -Subnet $subnet -PrivateLinkServiceConnection (New-AzPrivateLinkServiceConnection -Name conn1 -PrivateLinkServiceId $acct.Id -GroupId blob)
New-AzBastion -ResourceGroupName rg1 -Name bas1 -PublicIpAddressRgName rg1 -PublicIpAddressName pip-bastion -VirtualNetworkRgName rg1 -VirtualNetworkName vnet1 -Sku Standard
```

||Service endpoint|Private endpoint|
|---|---|---|
|IP used|service **public** IP (optimised route)|**private IP** in your subnet|
|On-prem access|❌|✅ (via VPN/ER)|
|Granularity|whole service type (per region)|**one specific resource**|
|DNS change|none|`privatelink.*` zone needed|
|Cost|free|charged|

**Private endpoint `--group-id`:** `blob` · `file` · `queue` · `table` · `dfs` · `web` (static site) · `sqlServer` · `vault` · `registry`

### ⚖️ Load balancer

```bash
az network lb create -g rg1 -n lb1 --sku Standard --public-ip-address pip-lb --frontend-ip-name fe1 --backend-pool-name be1
az network lb create -g rg1 -n ilb1 --sku Standard --vnet-name vnet1 --subnet app --private-ip-address 10.0.2.100 --frontend-ip-name fe1 --backend-pool-name be1   # internal
az network lb probe create -g rg1 --lb-name lb1 -n hp80 --protocol Http --port 80 --path /health --interval 5
az network lb rule create -g rg1 --lb-name lb1 -n http --protocol Tcp --frontend-port 80 --backend-port 80 --frontend-ip-name fe1 --backend-pool-name be1 --probe-name hp80 --load-distribution SourceIP --idle-timeout 4
az network lb inbound-nat-rule create -g rg1 --lb-name lb1 -n ssh-vm1 --protocol Tcp --frontend-port 50001 --backend-port 22 --frontend-ip-name fe1
az network lb outbound-rule create -g rg1 --lb-name lb1 -n out1 --frontend-ip-configs fe1 --address-pool be1 --protocol All
az network nic ip-config address-pool add -g rg1 --nic-name vm1-nic --ip-config-name ipconfig1 --lb-name lb1 --address-pool be1
```

```powershell
$fe = New-AzLoadBalancerFrontendIpConfig -Name fe1 -PublicIpAddress $pip
$be = New-AzLoadBalancerBackendAddressPoolConfig -Name be1
$hp = New-AzLoadBalancerProbeConfig -Name hp80 -Protocol Http -Port 80 -RequestPath /health -IntervalInSeconds 5 -ProbeCount 2
$rule = New-AzLoadBalancerRuleConfig -Name http -FrontendIpConfiguration $fe -BackendAddressPool $be -Probe $hp -Protocol Tcp -FrontendPort 80 -BackendPort 80 -LoadDistribution SourceIP
New-AzLoadBalancer -ResourceGroupName rg1 -Name lb1 -Location westeurope -Sku Standard -FrontendIpConfiguration $fe -BackendAddressPool $be -Probe $hp -LoadBalancingRule $rule
```

**`--load-distribution` (session persistence):** `Default` (5-tuple, none) · `SourceIP` (2-tuple, client IP) · `SourceIPProtocol` (3-tuple)

> [!tip] LB troubleshooting checklist Backend unhealthy → probe port/path wrong · **NSG not allowing `AzureLoadBalancer` service tag** / probe port · app not listening · VM not in backend pool. **Standard LB** is closed by default → needs NSG allow. Backend VMs in the same **VNet** (Standard: any VM/VMSS in that VNet). Probe health status in **Metrics → Health Probe Status**.

### 🔍 Network Watcher (troubleshooting)

```bash
az network watcher configure -g NetworkWatcherRG -l westeurope --enabled true
az network watcher test-ip-flow -g rg1 --vm vm1 --direction Inbound --protocol TCP --local 10.0.1.4:22 --remote 203.0.113.5:60000   # which NSG rule allows/denies
az network watcher show-next-hop -g rg1 --vm vm1 --source-ip 10.0.1.4 --dest-ip 8.8.8.8                                        # routing / UDR issues
az network watcher test-connectivity -g rg1 --source-resource vm1 --dest-address www.bing.com --dest-port 443                  # Connection troubleshoot
az network watcher packet-capture create -g rg1 --vm vm1 -n cap1 --storage-account acct1 --time-limit 300
az network watcher flow-log create -l westeurope -g rg1 -n fl-vnet1 --vnet vnet1 --storage-account acct1 --workspace law1 --traffic-analytics true   # VNet flow logs
az network watcher show-topology -g rg1
```

```powershell
$nw = Get-AzNetworkWatcher -Location westeurope
Test-AzNetworkWatcherIPFlow -NetworkWatcher $nw -TargetVirtualMachineId $vm.Id -Direction Inbound -Protocol TCP -LocalIPAddress 10.0.1.4 -LocalPort 22 -RemoteIPAddress 203.0.113.5 -RemotePort 60000
Get-AzNetworkWatcherNextHop -NetworkWatcher $nw -TargetVirtualMachineId $vm.Id -SourceIPAddress 10.0.1.4 -DestinationIPAddress 8.8.8.8
Test-AzNetworkWatcherConnectivity -NetworkWatcher $nw -SourceId $vm.Id -DestinationAddress www.bing.com -DestinationPort 443
New-AzNetworkWatcherPacketCapture -NetworkWatcher $nw -TargetVirtualMachineId $vm.Id -PacketCaptureName cap1 -StorageAccountId $acct.Id -TimeLimitInSeconds 300
```

|Problem|Tool|
|---|---|
|"Is traffic allowed/denied by an NSG? Which rule?"|**IP flow verify**|
|"Where does traffic go? Is a UDR hijacking it?"|**Next hop**|
|"Can VM A reach X on port Y (end-to-end)?"|**Connection troubleshoot**|
|Continuous reachability/latency monitoring|**Connection monitor**|
|All rules applied to a NIC|**Effective security rules**|
|Capture packets|**Packet capture** (needs Network Watcher agent ext.)|
|Log IP traffic|**VNet flow logs** (NSG flow logs retiring) + **Traffic analytics**|
|Visual map of resources|**Topology**|

> [!note] Reserved subnet names `AzureBastionSubnet` (/26+) · `GatewaySubnet` (/27 recommended) · `AzureFirewallSubnet` (/26) · `AzureFirewallManagementSubnet` (/26) · `RouteServerSubnet` (/27)

---

## 📊 6. Monitor & Maintain (10–15%)

### 📈 Azure Monitor — metrics, logs, alerts

```bash
az monitor log-analytics workspace create -g rg1 -n law1 -l westeurope --retention-time 30
az monitor diagnostic-settings create -n diag1 --resource $ID --workspace $LAWID --logs '[{"categoryGroup":"allLogs","enabled":true}]' --metrics '[{"category":"AllMetrics","enabled":true}]'
az monitor diagnostic-settings categories list --resource $ID
az monitor metrics list --resource $VMID --metric "Percentage CPU" --interval PT5M --aggregation Average
az monitor metrics list-definitions --resource $VMID
az monitor activity-log list -g rg1 --offset 7d --query "[].{op:operationName.localizedValue, who:caller}" -o table
az monitor log-analytics query -w <workspaceGuid> --analytics-query "Heartbeat | summarize max(TimeGenerated) by Computer"

az monitor action-group create -g rg1 -n ag-ops --short-name ops --action email admin admin@contoso.com --action sms oncall 353 851234567
az monitor metrics alert create -g rg1 -n cpu-high --scopes $VMID --condition "avg Percentage CPU > 80" --window-size 5m --evaluation-frequency 1m --severity 2 --action ag-ops
az monitor activity-log alert create -g rg1 -n vm-deleted --scope /subscriptions/<sub> --condition category=Administrative and operationName=Microsoft.Compute/virtualMachines/delete --action-group ag-ops
az monitor scheduled-query create -g rg1 -n vm-offline --scopes $LAWID --condition "count 'Heartbeat_q' < 1" --condition-query Heartbeat_q="Heartbeat | where TimeGenerated > ago(5m)" --window-size 5m --evaluation-frequency 5m --action-groups $AGID   # log alert
az monitor alert-processing-rule create -g rg1 -n maint-window --rule-type RemoveAllActionGroups --scopes /subscriptions/<sub> --schedule-start-datetime "2026-11-01 22:00:00" --schedule-end-datetime "2026-11-02 04:00:00"   # suppress during maintenance

az monitor data-collection rule create -g rg1 -n dcr-win -l westeurope --rule-file dcr.json
az monitor data-collection rule association create -n dcra1 --rule-id $DCRID --resource $VMID
az vm extension set -g rg1 --vm-name vm1 --name AzureMonitorLinuxAgent --publisher Microsoft.Azure.Monitor --enable-auto-upgrade true   # Windows: AzureMonitorWindowsAgent
```

```powershell
New-AzOperationalInsightsWorkspace -ResourceGroupName rg1 -Name law1 -Location westeurope -Sku PerGB2018 -RetentionInDays 30
New-AzDiagnosticSetting -Name diag1 -ResourceId $id -WorkspaceId $law.ResourceId -Log (New-AzDiagnosticSettingLogSettingsObject -Enabled $true -CategoryGroup allLogs) -Metric (New-AzDiagnosticSettingMetricSettingsObject -Enabled $true -Category AllMetrics)
Get-AzMetric -ResourceId $vm.Id -MetricName "Percentage CPU" -TimeGrain 00:05:00 -AggregationType Average
Get-AzActivityLog -ResourceGroupName rg1 -StartTime (Get-Date).AddDays(-7)
Invoke-AzOperationalInsightsQuery -WorkspaceId <guid> -Query "Heartbeat | take 10"

$email = New-AzActionGroupEmailReceiverObject -Name admin -EmailAddress admin@contoso.com
New-AzActionGroup -ResourceGroupName rg1 -Name ag-ops -ShortName ops -Location Global -EmailReceiver $email
$c = New-AzMetricAlertRuleV2Criteria -MetricName "Percentage CPU" -TimeAggregation Average -Operator GreaterThan -Threshold 80
Add-AzMetricAlertRuleV2 -Name cpu-high -ResourceGroupName rg1 -TargetResourceId $vm.Id -Condition $c -WindowSize 00:05:00 -Frequency 00:01:00 -ActionGroupId $ag.Id -Severity 2
```

|Alert type|Data source|Example|
|---|---|---|
|**Metric**|platform/custom metrics|CPU > 80%|
|**Log (scheduled query)**|Log Analytics KQL|no heartbeat in 5 min|
|**Activity log**|control-plane events / Service Health / Resource Health|VM deleted, service outage|
|**Smart detection**|App Insights|failure anomalies|

**Action group action types:** Email · SMS · Push (Azure app) · Voice · Azure Function · Logic App · Webhook / Secure webhook · Automation runbook · ITSM · Event Hub **Severity:** Sev0 Critical · Sev1 Error · Sev2 Warning · Sev3 Informational · Sev4 Verbose

### 🔎 KQL essentials

```kql
Heartbeat
| summarize LastSeen = max(TimeGenerated) by Computer
| where LastSeen < ago(15m)                                   // VMs that stopped reporting

Perf
| where TimeGenerated > ago(1h)
| where ObjectName == "Processor" and CounterName == "% Processor Time"
| summarize AvgCPU = avg(CounterValue) by bin(TimeGenerated, 5m), Computer
| render timechart

Event | where EventLevelName == "Error" | project TimeGenerated, Computer, Source, RenderedDescription | top 20 by TimeGenerated desc
Syslog | where SeverityLevel == "err" | count
AzureActivity | where OperationNameValue has "delete" | project TimeGenerated, Caller, ResourceGroup
InsightsMetrics | where Namespace == "LogicalDisk" and Name == "FreeSpacePercentage"
```

|Operator|Does|
|---|---|
|`where`|filter rows|
|`project` / `project-away`|select / drop columns|
|`extend`|add calculated column|
|`summarize … by`|aggregate (`count()` `avg()` `sum()` `max()` `dcount()`)|
|`bin(TimeGenerated, 5m)`|time buckets|
|`ago(1h)`|relative time|
|`take` / `limit` · `top N by`|sample / top N|
|`sort by` / `order by`|sort (desc default)|
|`distinct` · `count`|unique values · row count|
|`join kind=inner` · `union`|combine tables|
|`has` / `contains` / `==` / `=~`|term / substring / exact / case-insensitive|
|`render timechart` / `barchart` / `piechart`|visualise|

**Common tables:** `Heartbeat` · `Perf` · `Event` (Windows) · `Syslog` (Linux) · `AzureActivity` · `AzureDiagnostics` · `AzureMetrics` · `InsightsMetrics` (VM insights) · `VMConnection` · `Update`

### 🛟 Backup & Site Recovery

```bash
az backup vault create -g rg1 -n rsv1 -l westeurope
az backup vault backup-properties set -g rg1 -n rsv1 --backup-storage-redundancy GeoRedundant --cross-region-restore-flag true   # set BEFORE protecting items
az backup vault backup-properties set -g rg1 -n rsv1 --soft-delete-feature-state Enable
az backup policy list -g rg1 --vault-name rsv1 -o table
az backup protection enable-for-vm -g rg1 --vault-name rsv1 --vm vm1 --policy-name DefaultPolicy
az backup protection enable-for-azurefileshare -g rg1 --vault-name rsv1 --storage-account acct1 --azure-file-share share1 --policy-name FilePolicy
az backup protection backup-now -g rg1 --vault-name rsv1 --container-name vm1 --item-name vm1 --backup-management-type AzureIaasVM --retain-until 31-12-2026
az backup recoverypoint list -g rg1 --vault-name rsv1 --container-name vm1 --item-name vm1 --backup-management-type AzureIaasVM -o table
az backup restore restore-disks -g rg1 --vault-name rsv1 --container-name vm1 --item-name vm1 --rp-name <rp> --storage-account acct1 --target-resource-group rg-restore
az backup restore files mount-rp ...                       # file-level recovery (iSCSI script)
az backup protection disable -g rg1 --vault-name rsv1 --container-name vm1 --item-name vm1 --delete-backup-data false   # stop, retain data
az backup job list -g rg1 --vault-name rsv1 -o table

az dataprotection backup-vault create -g rg1 --vault-name bv1 -l westeurope --storage-settings datastore-type="VaultStore" type="LocallyRedundant"   # Backup vault
```

```powershell
$vault = New-AzRecoveryServicesVault -Name rsv1 -ResourceGroupName rg1 -Location westeurope
Set-AzRecoveryServicesBackupProperty -Vault $vault -BackupStorageRedundancy GeoRedundant
Set-AzRecoveryServicesVaultContext -Vault $vault               # (older pattern) or pass -VaultId everywhere
$pol = Get-AzRecoveryServicesBackupProtectionPolicy -Name DefaultPolicy -VaultId $vault.ID
Enable-AzRecoveryServicesBackupProtection -Policy $pol -Name vm1 -ResourceGroupName rg1 -VaultId $vault.ID
$cont = Get-AzRecoveryServicesBackupContainer -ContainerType AzureVM -FriendlyName vm1 -VaultId $vault.ID
$item = Get-AzRecoveryServicesBackupItem -Container $cont -WorkloadType AzureVM -VaultId $vault.ID
Backup-AzRecoveryServicesBackupItem -Item $item -VaultId $vault.ID -ExpiryDateTimeUTC (Get-Date).AddDays(30)
$rp = Get-AzRecoveryServicesBackupRecoveryPoint -Item $item -VaultId $vault.ID
Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -StorageAccountName acct1 -StorageAccountResourceGroupName rg1 -VaultId $vault.ID
Get-AzRecoveryServicesBackupJob -VaultId $vault.ID
```

|Vault|Protects|
|---|---|
|**Recovery Services vault**|Azure VMs · Azure Files · SQL/SAP HANA in VM · on-prem via **MARS** agent / MABS / DPM · **Azure Site Recovery**|
|**Backup vault** (`az dataprotection`)|Azure **Blobs** · Azure **Managed Disks** · PostgreSQL · AKS|

> [!warning] Backup traps Vault must be in the **same region** as the VM. Storage redundancy (LRS/GRS/ZRS) **locked once items are protected**. Soft-deleted backups kept **14 days** (extra). CLI `--retain-until` format = **dd-mm-yyyy**. Cross Region Restore requires **GRS**. **ASR** (replication/failover to another region) is configured in a **Recovery Services vault** in the **target** region; run **test failover** into an isolated VNet.

### 🧩 Other maintenance commands

```bash
az resource list -g rg1 -o table
az resource show --ids $ID
az group list --query "[?location=='westeurope']" -o table
az monitor activity-log list --resource-id $ID              # who changed what
az vm assess-patches -g rg1 -n vm1                          # Azure Update Manager
az vm install-patches -g rg1 -n vm1 --maximum-duration PT2H --reboot-setting IfRequired --classifications-to-include-linux Critical Security
```

```powershell
Get-AzResource -ResourceGroupName rg1 | Format-Table
Invoke-AzVMPatchAssessment -ResourceGroupName rg1 -VMName vm1
```

---

## 🆚 7. Side-by-Side Quick Lookup

|Task|Azure CLI|PowerShell|
|---|---|---|
|Sign in|`az login`|`Connect-AzAccount`|
|Switch subscription|`az account set --subscription`|`Set-AzContext -Subscription`|
|Create RG|`az group create`|`New-AzResourceGroup`|
|Delete RG|`az group delete --yes`|`Remove-AzResourceGroup -Force`|
|Move resources|`az resource move`|`Move-AzResource`|
|Lock|`az lock create --lock-type`|`New-AzResourceLock -LockLevel`|
|Tag (merge)|`az tag update --operation Merge`|`Update-AzTag -Operation Merge`|
|Assign role|`az role assignment create`|`New-AzRoleAssignment`|
|Custom role|`az role definition create`|`New-AzRoleDefinition -InputFile`|
|Assign policy|`az policy assignment create`|`New-AzPolicyAssignment`|
|Remediate|`az policy remediation create`|`Start-AzPolicyRemediation`|
|Management group|`az account management-group create`|`New-AzManagementGroup`|
|Create user|`az ad user create`|`New-MgUser` / `New-AzADUser`|
|Create group|`az ad group create`|`New-MgGroup` / `New-AzADGroup`|
|Storage account|`az storage account create`|`New-AzStorageAccount`|
|Change redundancy|`az storage account update --sku`|`Set-AzStorageAccount -SkuName`|
|Rotate key|`az storage account keys renew`|`New-AzStorageAccountKey`|
|Container|`az storage container create`|`New-AzStorageContainer`|
|Upload blob|`az storage blob upload`|`Set-AzStorageBlobContent`|
|SAS|`az storage … generate-sas`|`New-AzStorage…SASToken`|
|File share|`az storage share-rm create`|`New-AzRmStorageShare`|
|Deploy template|`az deployment group create`|`New-AzResourceGroupDeployment`|
|Preview template|`az deployment group what-if`|`New-AzResourceGroupDeployment -WhatIf`|
|Export RG template|`az group export`|`Export-AzResourceGroup`|
|Create VM|`az vm create`|`New-AzVM`|
|Deallocate VM|`az vm deallocate`|`Stop-AzVM`|
|Resize VM|`az vm resize`|`Update-AzVM` (after setting VmSize)|
|Redeploy VM|`az vm redeploy`|`Set-AzVM -Redeploy`|
|Run script in VM|`az vm run-command invoke`|`Invoke-AzVMRunCommand`|
|Add data disk|`az vm disk attach`|`Add-AzVMDataDisk` + `Update-AzVM`|
|Snapshot|`az snapshot create`|`New-AzSnapshot`|
|Scale set|`az vmss create` / `scale`|`New-AzVmss` / `Update-AzVmss`|
|Container instance|`az container create`|`New-AzContainerGroup`|
|Web app|`az webapp create`|`New-AzWebApp`|
|Swap slot|`az webapp deployment slot swap`|`Switch-AzWebAppSlot`|
|VNet|`az network vnet create`|`New-AzVirtualNetwork`|
|Add subnet|`az network vnet subnet create`|`Add-AzVirtualNetworkSubnetConfig` + `Set-AzVirtualNetwork`|
|Peering|`az network vnet peering create`|`Add-AzVirtualNetworkPeering`|
|NSG rule|`az network nsg rule create`|`New-AzNetworkSecurityRuleConfig`|
|Route|`az network route-table route create`|`Add-AzRouteConfig`|
|Effective NSG|`az network nic list-effective-nsg`|`Get-AzEffectiveNetworkSecurityGroup`|
|Effective routes|`az network nic show-effective-route-table`|`Get-AzEffectiveRouteTable`|
|IP flow verify|`az network watcher test-ip-flow`|`Test-AzNetworkWatcherIPFlow`|
|Next hop|`az network watcher show-next-hop`|`Get-AzNetworkWatcherNextHop`|
|Connectivity|`az network watcher test-connectivity`|`Test-AzNetworkWatcherConnectivity`|
|DNS zone|`az network dns zone create`|`New-AzDnsZone`|
|Private DNS link|`az network private-dns link vnet create`|`New-AzPrivateDnsVirtualNetworkLink`|
|Metric alert|`az monitor metrics alert create`|`Add-AzMetricAlertRuleV2`|
|Action group|`az monitor action-group create`|`New-AzActionGroup`|
|Diagnostic setting|`az monitor diagnostic-settings create`|`New-AzDiagnosticSetting`|
|Backup vault (RSV)|`az backup vault create`|`New-AzRecoveryServicesVault`|
|Enable VM backup|`az backup protection enable-for-vm`|`Enable-AzRecoveryServicesBackupProtection`|
|On-demand backup|`az backup protection backup-now`|`Backup-AzRecoveryServicesBackupItem`|

---

## 🎯 8. Exam Quick-Reference (numbers & rules)

> [!tip] Command traps — one-liners
> 
> - `az vm stop` → **still billed** · `az vm deallocate` / `Stop-AzVM` → not billed · `Stop-AzVM -StayProvisioned` → billed
> - PS edits need commit: `Set-AzVirtualNetwork` · `Set-AzNetworkSecurityGroup` · `Set-AzRouteTable` · `Update-AzVM` · `Set-AzNetworkInterface`
> - `New-AzStorageAccountKey` = **regenerate** a key
> - `--tags` on `az group update` / `New-AzTag` **replace**; use `Merge` to add
> - Deployment `--mode Complete` **deletes** unlisted resources; default **Incremental**
> - Peering: **create both sides**; **non-transitive**
> - Disk SKU change / resize to size outside current cluster / add to availability set → **deallocate** (avset = recreate)
> - Policy remediation needs **managed identity** + DINE/Modify effect
> - Data-plane access with Entra = **Storage Blob/File Data** roles, not Owner/Contributor
> - Licence assignment needs **UsageLocation**
> - AzureAD / MSOnline modules **retired** → Graph (`*-Mg*`)

|Limit / rule|Value|
|---|---|
|NSG rule priority|**100–4096** (lower wins) · defaults 65000–65500|
|Reserved IPs per subnet|**5** (first 4 + last) · smallest subnet **/29**|
|AzureBastionSubnet|**/26** or larger|
|GatewaySubnet|**/27** recommended|
|Availability set|max **3** FD · max **20** UD (default 5)|
|VMSS (Uniform)|up to **1,000** instances (600 custom image)|
|VM name length|Windows **15** · Linux **64**|
|Storage account name|**3–24**, lowercase + numbers, globally unique|
|Blob/container soft delete|**1–365** days|
|Min days before early-deletion fee|Cool **30** · Cold **90** · Archive **180**|
|Archive rehydrate|Standard **≤15 h** · High **<1 h** (<10 GB)|
|Azure Files share size|up to **100 TiB** (large file shares)|
|Share snapshots|**200** per share|
|SMB / NFS ports|**445** / **2049**|
|Tags|**50** per resource · name 512 (storage 128) · value 256|
|Mgmt group depth|**6** levels (excl. root) · **10,000** MGs/tenant|
|Role assignments|**4,000** per subscription|
|Custom roles|**5,000** per tenant|
|Activity log retention|**90** days (longer → diagnostic setting to LAW/storage)|
|Log Analytics retention|default **30** days · interactive up to **730** · total up to **12 yrs**|
|Recovery Services soft delete|**14** days|
|App Service slots|Standard **5** · Premium/Isolated **20**|
|Azure-provided DNS IP|**168.63.129.16**|
|Policy evaluation|new assignment ~**30 min** · standard cycle **24 h**|