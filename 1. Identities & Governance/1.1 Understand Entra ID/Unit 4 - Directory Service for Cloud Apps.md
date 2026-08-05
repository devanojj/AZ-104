**Single Tenant across Services**
Rather than creating separate directories, multiple Microsoft cloud services (M365, Azure, Intune, Dynamics 365) share a single Microsoft Entra tenant for centralised identity management.

**SSO & Identity Integration**
Entra ID acts as a central identity provider, supporting Single Sign-On (SSO) with external providers (Google, Facebook, Yahoo) and on-premises AD DS.

**Azure App Service Authentication**
Entra ID authentication can be enabled directly on Web Apps using the Authentication/Authorization blade in the Azure portal without complex code.

**Deployment Slot Control**
Authentication settings can be configured granularly per individual deployment slot.