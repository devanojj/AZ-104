
- **CanNotDelete lock** → blocks deletion
- **Owner** can **delete the lock** because they have `Microsoft.Authorization/locks/delete`.
- **CanNotDelete + Owner:** Modify ✅ | Delete resource ❌ | Delete lock ✅
- **ReadOnly lock** → blocks both **modify and delete** operations.

*Owner can then delete lock then delete resource*