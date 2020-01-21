# Manage identities

## Manage Azure Active Directory (AD)

* add custom domains
* Azure AD Join
  * Requires:
    * Azure AD + Subscription
    * Windows 10 (Pro/Ent)
  * Admin Tasks
  * User Tasks
    * [Register Windows 10 Device – BYOD](https://docs.microsoft.com/en-us/azure/active-directory/user-help/user-help-register-device-on-network)
    * Join Corp Device
      * Settings, Account, Access Work or School
  * Verify
* configure self-service password reset
* manage multiple directories

* Enterprise State Roaming - With Windows 10, Azure Active Directory (Azure AD) users gain the ability to securely synchronize their user settings and application settings data to the cloud. Enterprise State Roaming provides users with a unified experience across their Windows devices and reduces the time needed for configuring a new device. Enterprise State Roaming operates similar to the standard consumer settings sync that was first introduced in Windows 8.
  * Requires:
    * Azure AD Premium or Enterprise Mobility + Security (EMS) license
    * Windows 10
    * Azure AD Domain Join
      * This requires that you deploy Azure AD Connect to allow you to synchronize objects in your on-prem AD DS domain with an Azure AD tenant.
  * 3 regions: NA, EMEA, APAC
  * Not replicated across
  * Country/Regioin set on attribute and cannot be changed after
  * Retained until deleted or becomes "stale"

## Manage Azure AD objects (users, groups, and devices)

* create users and groups
* manage user and group properties
* manage device settings
* perform bulk user updates
  * PowerShell
    * Import a CSV
    * Export from HR or SQL
  * Process
    * Connect-AzureAD (MSonline still works, mainly used for Office 365 integration)
    * Define Variables
* manage guest accounts

| Rules | Desc | Code Example |
|----|----|----|
| Direct Reports | You can create a group containing all direct reports of a manager. When the manager's direct reports change in the future, the group's membership is adjusted automatically. | `Direct Reports for "{objectID_of_manager}"` |
| All Users | You can create a group containing all users within a tenant using a membership rule. When users are added or removed from the tenant in the future, the group's membership is adjusted automatically. | `user.objectid -ne null` |
| Extensions | Extension attributes and custom extension properties are supported as string properties in dynamic membership rules. Extension attributes are synced from on-premises Window Server AD and take the format of "ExtensionAttributeX", where X equals 1 - 15. | `(user.extensionAttribute15 -eq "Marketing")` |
| Custom Extensions | Custom extension properties are synced from on-premises Windows Server AD or from a connected SaaS application  <br/>`[GUID]` is the unique identifier in Azure AD for the application that created the property in Azure AD<br/>`[Attribute]` is the name of the property as it was created| `user.extension_[GUID]_[Attribute]` |

![](screenshots/conditional-access-overview-how-it-works.png)

| Azure Active Directory Conditional Access |  if a user wants to access a resource, then they must complete an action |
|----|----|
| Users and groups | In your policy, you can either select All users or select specific users and groups. When you Select users and groups, you can set the following options:<br/>• All guest users<br/>• Directory roles<br/>• Users and groups |
| Cloud apps and actions | Mandatory in a Conditional Access policy. In your policy, you can either select:<br/>• All cloud apps or<br/>• specify apps with Select apps.|
| Sign-in risk | An indicator of the likelihood (high, medium, or low) that a sign-in wasn't made by the legitimate owner of a user account. To use this condition, you need to have Azure Active Directory Identity Protection enabled. |
| Device platforms | Operating system on a client. Supports the following device platforms:<br/>• Android<br/>• iOS<br/>• Windows Phone<br/>• Windows<br/>• macOS |
| Device state | The device state condition excludes hybrid Azure AD joined devices and devices marked as compliant from a Conditional Access policy. This condition is useful when a policy should apply only to an unmanaged device to provide additional session security. For example, only enforce the Microsoft Cloud App Security session control when a device is unmanaged. |
| Locations | Based on where a connection was attempted. Common use cases for this condition are policies with the following protections:<br/>• Require multi-factor authentication for users accessing a service when they're off the corporate network.<br/>• Block access for users accessing a service from specific countries or regions.|
| Client apps | By default, a Conditional Access policy applies to the following apps:<br/>• Browser apps<br/>• Mobile and desktop apps using modern authentication<br/>Additionally, you can target a policy to specific client apps that are not using modern authentication, for example:<br/>• Exchange ActiveSync clients<br/>• Other clients<br/>Common use cases for this condition are policies with the following requirements:<br/>• Require a managed device<br/>• Block legacy authentication<br/>You can only select Exchange ActiveSync clients if:<br/>• Microsoft Office 365 Exchange Online is the only cloud app you've selected.<br/>• You don't have other conditions configured in a policy. However, you can narrow down the scope of this condition to apply only to supported platforms.|

## Implement and manage hybrid identities

* [install Azure AD Connect](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-install-custom), including password hash and pass-through synchronization
* use Azure AD Connect to configure federation with on-premises Active Directory Domain Services (AD DS)
* manage Azure AD Connect
* manage password sync and password writeback

| AD Connect Components | AD Connect Sync Features |
|----|----|
| Synchronization Services | Filtering |
| Active Directory Federation Services (optional) | Password Hash Sync (on-prem to cloud) |
| Health Monitoring | Password writeback<br/>Device writeback<br/>Prevent accidental deletes<br/>Automatic upgrade |

| **Password Sync Options** | desc |
|----|----|
| Password Sync | Ensures user passwords are the same in both directories (AD DS and Azure AD) |
| Passthrough Authentication | Easy method to keep users and passwords aligned. When a user logs into Azure AD, the request is forwarded to AD DS. Essentially, a single source. |
| AD FS | Use AD Federation Services server to fully federate across AD DS and Azure AD, along with other services. |

| **Domain Services** | |
|----|----|
| Azure AD (AAD) | • Modern AD service built directly for the cloud<br/>• Often the same as O365 directory service<br/>• Can sync with On-prem directory service<br/>• Enterprise Identity Solution<br/>• Single Sign-on<br/>• Multi-factor Authentication (MFA)<br/>• Self-service (i.e. password reset) |
| Active Directory Domain Services (ADDS) | • Legacy Active Directory since Windows 2000<br/>• Traditional Kerberos and LDAP functionality<br/>• Deployed on Windows OS usually on VMs |
| Azure Active Directory Domain Services (AADDS) | • Provides managed domain services<br/>• Allows you to consume domain services w/o the need to patch and maintain domain controllers on IaaS<br/>All supported: <br/>• Domain Join<br/>• Group Policy<br/>• LDAP<br/>• Kerberos<br/>• NTLM |

| AD Scenario | |
|----|----|
| Cloud-only | • Users and Groups are managed in Azure Active Directory only<br/> • Azure AD stores the password (encrypted) |
| Hybrid AD w/password sync | • On-premises Active Directory is the « master » <br/> • Users and Groups are synchronized to Azure Active Directory using ADConnect (or MIM or 3rd party)<br/> • Password is synced encrypted out of ADConnect (or MIM or 3rd party) |
| Hybrid AD w/Federation | • Relies on ADConnect to synchronize objects<br/> • ADFS or 3rd party Federation engine, running on-premises or in Azure VMs (with S2S or ER)|
| Hybrid AD w/PassThrough Authentication | • Relies on ADConnect to synchronize objects<br/> • Instead of ADFS federation, deploy the PTA agents on on-premises servers (ADDS DCs)|

**Active Directory Pricing Details**

| Common Features | Free | Basic | Premium P1 | Premium P2 |
|----|----|----|----|----|
| Directory Objects | 500,000 Object Limit | No Object Limit | No Object Limit | No Object Limit |
| Single Sign-On (SSO) | 10 apps per user | 10 apps per user | No Limit | No Limit |
| Group-based access mgmt/provisioning | no | yes | yes | yes |
| Self-Service Password reset for cloud users | no | yes | yes | yes |
| Company Branding | no | yes | yes | yes |
| Application Proxy | no | yes | yes | yes |
| SLA | no | yes | yes | yes |
| Self-Service Password reset/change/unlock with on-prem write-back | no | no | yes | yes |
| Device objects two-way sync b/w on-prem directories and Azure AD (Device write-back) | no | no | yes | yes |
| MFA (Cloud and on-prem (MFA server)) | limited | limited | yes | yes |
| Identity Protection | no | no | no | yes |
| Privileged Identity Management | no | no | no | yes |
| 3rd Party MFA Partner Integration | no | no | no | yes |
| SharePoint Limited Access | no | no | no | yes |



## Implement multi-factor authentication (MFA)

* configure user accounts for MFA
* enable MFA by using bulk update
* configure fraud alerts
* configure bypass options
* configure Trusted IPs
* configure verification methods

----
**Enable Azure MFA by changing user state**

_User accounts in Azure Multi-Factor Authentication have the following three distinct states:_

|Status | Description | Non-browser apps affected | Browser apps affected | Modern authentication affected|
|----|----|----|----|----|
|Disabled | The default state for a new user not enrolled in Azure MFA. | No | No | No|
|Enabled | The user has been enrolled in Azure MFA, but has not registered. They receive a prompt to register the next time they sign in. | No. They continue to work until the registration process is completed. | Yes. After the session expires, Azure MFA registration is required. | Yes. After the access token expires, Azure MFA registration is required.|
|Enforced | The user has been enrolled and has completed the registration process for Azure MFA. | Yes. Apps require app passwords. | Yes. Azure MFA is required at login. | Yes. Azure MFA is required at login.|

```Powershell
# Modifies a user in Azure Active Directory.

Set-MsolUser
   [-ImmutableId <String>]
   [-ObjectId <Guid>]
   [-UserPrincipalName <String>]
   [-BlockCredential <Boolean>]
   [-City <String>]
   [-Country <String>]
   [-Department <String>]
   [-DisplayName <String>]
   [-Fax <String>]
   [-FirstName <String>]
   [-LastName <String>]
   [-LastPasswordChangeTimestamp <DateTime>]
   [-MobilePhone <String>]
   [-Office <String>]
   [-PasswordNeverExpires <Boolean>]
   [-PhoneNumber <String>]
   [-PostalCode <String>]
   [-PreferredDataLocation <String>]
   [-PreferredLanguage <String>]
   [-SoftDeletionTimestamp <DateTime>]
   [-State <String>]
   [-StreetAddress <String>]
   [-StrongPasswordRequired <Boolean>]
   [-Title <String>]
   [-UsageLocation <String>]
   [-AlternateEmailAddresses <String[]>]
   [-StrongAuthenticationMethods <StrongAuthenticationMethod[]>]
   [-AlternateMobilePhones <String[]>]
   [-StrongAuthenticationRequirements <StrongAuthenticationRequirement[]>] # Specifies an array of strong authentication requirements.
   [-StsRefreshTokensValidFrom <DateTime>]
   [-UserType <UserType>]
   [-TenantId <Guid>]
   [<CommonParameters>]
```

