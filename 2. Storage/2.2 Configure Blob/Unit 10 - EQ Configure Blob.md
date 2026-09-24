**Your company is storing large datasets for short-term backup purposes, which are rarely accessed. You need to optimise storage costs while ensuring data is immediately available when needed. Which tier should be utilised?**
- Hot tier
- Archive tier
- *Cool tier (correct)*

**Which of the following is a primary benefit of enabling object replication in Azure Blob Storage?**
- Replicates blob snapshots to the destination account.
- Automatically transitions data to the Archive tier.
- *Reduces latency for read requests by consuming data from a closer region. (correct)*

**Your organisation wants to optimise the storage costs for a dataset that is accessed frequently in the first week, occasionally in the second week, and hardly after a month. Which lifecycle management strategy should be implemented to achieve this goal?**
- *Transition to Hot tier for the first week, Cool tier for the second week, and Archive tier after a month. (correct)*
- Keep the data in Hot tier for the first month and transition to Cold tier thereafter.
- Transition to Cool tier for the first month and delete data after a month.

**How does implementing a lifecycle management policy in Azure Blob Storage contribute to optimising performance and scalability?**
- By enabling instant data replication across multiple geographic locations for improved availability.
- *By automatically transitioning data to appropriate access tiers based on usage patterns, reducing costs and improving access times. (correct)*
- By providing granular access control to different types of blob data.

**In designing a data distribution strategy using Azure Blob Storage, how can object replication optimise storage policy costs post-replication?**
- By compressing replicated data to reduce storage costs.
- By automatically deleting replicated data after a certain period.
- *By allowing lifecycle management policies to move data to the Archive tier after replication. (correct)*

**Which Azure Blob Storage tier would be most appropriate for storing data that is accessed frequently for analytics and reporting purposes?**
- Cool tier
- Archive tier
- *Hot tier (correct)*

**When designing a lifecycle management policy in Azure Blob Storage, what is the primary purpose of configuring the 'If - Then' rules?**
- *To set conditions and actions for transitioning or deleting blob data based on its age. (correct)*
- To configure access levels for containers and blobs.
- To define replication policies between different storage accounts.

**A company observes that their data retrieval costs have increased significantly. They currently use the Hot tier for data that is read infrequently. What should they do to optimise costs while maintaining accessibility?**
- Switch to the Cold tier (selected)
- Switch to the Archive tier
- *Switch to the Cool tier (correct)*

**A company needs to ensure data availability in case of regional outages. Which Azure Blob Storage feature should they implement to achieve this?**
- *Object replication to asynchronously copy blobs across regions. (correct)*
- Cold tier storage for long-term data retention.
- Blob snapshots to periodically save the state of blobs.