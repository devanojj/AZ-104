*Question 1*
~~You need to delegate administrative permissions to allow a helpdesk user to reset passwords only for employees in the Marketing department. You want to ensure the administrator cannot reset passwords for users outside this department. Which feature should you implement?~~
- ~~A.~~
    
    ~~Administrative Units~~
    
    ~~Correct answer~~
    
    ~~Administrative units allow you to subdivide an organization into smaller boundaries and delegate specific Azure AD/Entra ID roles scoped only to the members of that unit.~~
- ~~B.~~
    
    ~~Custom RBAC Roles at Subscription Scope~~
    
    ~~(Your answer)~~
    
    ~~Incorrect~~
    
    ~~RBAC roles at the subscription level apply to Azure Resource Manager resources, not directory-level user management.~~
- ~~C.~~
    
    ~~Management Groups~~
- ~~D.~~
    
    ~~Application Security Groups~~

*Question 3*
~~An administrator applies a `ReadOnly` resource lock to an Azure Storage Account. What is the effect on access keys and data operations?~~
- ~~A.~~
    
    ~~All existing shared access signatures become immediately invalid.~~
- ~~B.~~
    
    ~~Users can write new blob items using existing keys.~~
- ~~C.~~
    
    ~~Users cannot list storage account access keys.~~
    
    ~~Correct answer~~
    
    ~~Listing storage account access keys is a POST action (`listKeys`) that returns sensitive credentials; ReadOnly locks prevent POST operations on the resource.~~
- ~~D.~~
    
    ~~The storage account is automatically converted to Read-Access Geo-Redundant Storage.~~
    
    ~~(Your answer)~~
    
    ~~Incorrect~~
    
    ~~Resource locks only restrict control-plane management operations and do not alter the replication SKU configuration.~~

