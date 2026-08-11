## Check your knowledge

1. 

What is a role definition in Azure?

A collection of permissions with a name that is assignable to a user, group, or application.

A role definition in Azure is a collection of permissions with a name that you can assign to a user, group, or application.

The collection of users, groups, or applications that have permissions to a role.

The binding of a role to a security principal at a specific scope to grant access.

2. 

Suppose an administrator wants to assign a role to allow a user to create and manage Azure resources but not be able to grant access to others. Which of the following built-in roles would support this?

Owner

Contributor

A contributor can create and manage all types of Azure resources, but they can't grant access to other users.

Reader

User Access Administrator

3. 

What is the inheritance order for scope in Azure?

Management group, Resource group, Subscription, Resource

Management group, Subscription, Resource group, Resource

The inheritance order for scope is Management group, Subscription, Resource group, Resource. For example, if you assigned a Contributor role to a group at the Subscription scope level, it'll be inherited by all Resource groups and Resources.

Subscription, Management group, Resource group, Resource

Subscription, Resource group, Management group, Resource