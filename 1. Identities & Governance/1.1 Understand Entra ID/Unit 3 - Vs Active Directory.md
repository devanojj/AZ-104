**[[AD]]**
- Hierarchical X.500-based structure.
- Uses Domain Name System (DNS) for locating resources such as domain controllers.
- Query and manage using Lightweight Directory Access Protocol (LDAP) calls.
- Uses the Kerberos protocol for authentication.
- Uses OUs and [[GPO]]s for management.
- Includes computer objects, representing computers that join an Active Directory domain.
- Uses trusts between domains for delegated management.


**Entra**
- HTTP (port 80) and HTTPS (port 443) communications.
- Multi-tenant directory service.
- Users and groups are created in a flat structure, and there are no OUs or GPOs.
- You can't query Microsoft Entra ID by using LDAP; instead, Microsoft Entra ID uses the REST API over HTTP and HTTPS.
- Microsoft Entra ID doesn't use Kerberos authentication
- It uses HTTP and HTTPS protocols such as SAML, WS-Federation, and OpenID Connect for authentication, and uses OAuth for authorization.
- Microsoft Entra ID includes federation services, and many third-party services such as Facebook are federated with and trust Microsoft Entra ID.



