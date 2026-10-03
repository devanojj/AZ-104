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




Question 8

You are evaluating VM pricing models. Your workload is a non-critical, interruptible batch processing job that can tolerate short downtimes. Which pricing model offers the deepest discounts?

- A.
    
    Pay-as-you-go rates
    
- B.
    
    Spot instances
    
    (Your answer)
    
    Correct answer
    
    Spot instances provide significant cost savings for interruptible workloads that can handle potential preemption.
    
- C.
    
    Reserved instances
    
- D.
    
    Hybrid benefit pricing


Question 9

You need to host an application across two different availability zones within the same region to ensure high availability. Which feature allows you to group virtual machines specifically to ensure they are physically separated?

- A.
    
    Availability sets
    
    (Your answer)
    
    Incorrect
    
    Availability sets protect VMs within a single datacenter using fault and update domains, not across multiple zones.
    
- B.
    
    Availability zones
    
    Correct answer
    
    Availability zones are physically separate locations within an Azure region designed to protect against datacenter failures.
    
- C.
    
    Proximity placement
    
- D.
    
    Update domains


You have a Management Group that contains several subscriptions. You need to ensure a user can view all resources across these subscriptions but cannot modify them. Which scope should you assign the `Reader` role to?

- A.
    
    Subscription
    
    (Your answer)
    
    Incorrect
    
    Assigning at the subscription level would require individual assignments for every subscription in the group.
    
- B.
    
    Subscription Root
    
- C.
    
    Management Group
    
    Correct answer
    
    Assigning a role at the Management Group scope grants the permission to all subscriptions contained within that hierarchy.
    
- D.
    
    Resource Group

  
5 / 20

2 

3 

Question 5

You want to ensure that your Azure Storage Account is accessible only from a specific subnet within your virtual network. What configuration should you perform?

- A.
    
    Service Endpoints
    
    Correct answer
    
    Service Endpoints secure the path between the VNet and the storage service, allowing access only from the specified subnet.
    
- B.
    
    IP Firewall
    
- C.
    
    Network Security Group
    
    (Your answer)
    
    Incorrect
    
    NSGs filter traffic at the VM interface level and cannot restrict storage account network access rules directly.
    
- D.
    
    Private Endpoints


  
9 / 20

3 

6 

Question 9

You are deploying a set of virtual machines that require maximum uptime. You want to ensure they are physically separated across independent hardware. Which configuration should you choose?

- A.
    
    Scale Sets
    
- B.
    
    Availability Zones
    
    Correct answer
    
    Availability Zones provide physically separate power, cooling, and networking within a single Azure region.
    
- C.
    
    Resource Groups
    
- D.
    
    Availability Sets
    
    (Your answer)
    
    Incorrect
    
    Availability Sets protect within a single datacenter, providing less isolation than Availability Zones.




Question 12

You are hosting a web app in an App Service Plan. You need to scale the application to handle a sudden, high-traffic surge that exceeds the capacity of the current plan. What strategy is this?

- A.
    
    Scale up
    
    (Your answer)
    
    Incorrect
    
    Scaling up involves increasing the power of the instances (CPU/RAM), which is better for compute-intensive tasks.
    
- B.
    
    Provisioning
    
- C.
    
    Scale out
    
    Correct answer
    
    Scaling out involves adding more instances to handle increased load, which is appropriate for high traffic surges.
    
- D.
    
    Vertical scaling


A Network Security Group (NSG) has a default rule. What happens to inbound traffic that does not match any custom security rules?

- A.
    
    Implicitly denied
    
    Correct answer
    
    NSGs have default rules that implicitly deny all inbound traffic unless specifically allowed by a higher-priority rule.
    
- B.
    
    Implicitly allowed
    
- C.
    
    Dropped silently
    
    (Your answer)
    
    Incorrect
    
    While the traffic is blocked, 'Implicitly denied' is the standard terminology for default NSG behavior.
    
- D.
    
    Forwarded onwards



Question 17

You are writing a Kusto Query Language (KQL) query to find events in a Log Analytics Workspace. You want to filter for errors that occurred within the last 24 hours. Which operator should you use?

- A.
    
    within(24h)
    
- B.
    
    time(24h)
    
- C.
    
    ago(24h)
    
    Correct answer
    
    The `ago()` function combined with a time span is the standard way to filter time-based events in KQL.
    
- D.
    
    last(24h)
    
    (Your answer)
    
    Incorrect
    
    `last()` is not an operator used for time-range filtering in KQL log queries.

ou are deploying Azure File Sync. You need to configure a Sync Group. What is the primary relationship between the Sync Group and the registered server?

- A.
    
    Many-to-many relationship.
    
    Correct answer
    
    Sync Groups can contain endpoints from multiple servers, enabling synchronization across different physical endpoints.
    
- B.
    
    Server is parent of group.
    
- C.
    
    Group is specific to server.
    
    (Your answer)
    
    Incorrect
    
    Sync Groups are cloud-based configurations that define sync topologies, not server-specific objects.
    
- D.
    
    1:1 mapping only.


 ou have a Private DNS Zone linked to VNet1. You create a new VNet, VNet2, and you need VNet2 to resolve private DNS records from that same zone. What must you do?

- A.
    
    Add VNet2 link.
    
    (Your answer)
    
    Correct answer
    
    You must explicitly add a virtual network link in the Private DNS Zone to allow VNet2 to resolve the records.
    
- B.
    
    Peering VNet1-VNet2.
    
- C.
    
    Public IP assignment.
    
- D.
    
    Export zone records.


You have an Azure Policy that denies the creation of unmanaged disks. You need to verify if an existing storage account containing unmanaged disks will be affected. What is the standard behavior?

- A.
    
    Existing trigger alerts.
    
    (Your answer)
    
    Incorrect
    
    While you can audit them, the 'Deny' effect itself does not automatically turn into an alert for non-compliant resources.
    
- B.
    
    Existing are modified.
    
- C.
    
    Existing stay active.
    
    Correct answer
    
    Azure Policy's 'Deny' effect prevents the creation or _modification_ of resources to a non-compliant state, but it does not retroactively delete existing ones.
    
- D.
    
    Existing are deleted.


You are hosting a web app in an App Service Plan. You notice the CPU usage is consistently high. What is the best strategy to scale out to handle the surge?

- A.
    
    Add plan instances.
    
    Correct answer
    
    Scaling out (increasing instance count) distributes the traffic load across more instances, handling high traffic.
    
- B.
    
    Disable auto-scaling.
    
    (Your answer)
    
    Incorrect
    
    Disabling auto-scaling would prevent the system from reacting to traffic spikes effectively.
    
- C.
    
    Reduce max instances.
    
- D.
    
    Upgrade instance size.