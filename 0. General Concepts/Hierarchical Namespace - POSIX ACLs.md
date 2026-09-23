*Portable Operating System Interface*
Standard by IEEE programs run on OS without needing changes

ACL relates to who can access what 
For POSIX ACL this you need to enable namespace

Why? 
Control access at file / directory level

/data
//finance
-> salaries.csv
//marketing
-> campaigns.csv

- Finance team → access to `//finance`
- Marketing team → access to `//marketing`

*Used for [[Azure Data Lake Storage]]*


