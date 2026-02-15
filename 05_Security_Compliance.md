### **Document 05: Security & Compliance**

**File Name:** `05_Security_Compliance.md`

## **1. Overview**

This document outlines the security configuration for the User Lifecycle Agent. The system requires specific Microsoft Graph permissions to manage user profiles, security groups, and license assignments.

## **2. Microsoft Graph API Permissions**

To perform the required lifecycle actions (adding users to security groups, assigning licenses, and managing profiles), the following permissions must be granted to the Service Principal.

### **2.1 Application Permissions (Admin Consent Required)**

These permissions allow the Power Automate flows to perform background actions without a signed-in user.

* **`Directory.Read.All`**: Allows the system to read directory data to verify existing objects.
* **`Directory.ReadWrite.All`**: Required for adding users to security groups and managing group memberships.

### **2.2 Delegated Permissions**

These permissions are utilized for user-centric actions and identity verification.

* **`User.ReadWrite.All`**: Allows the agent to read and write full user profiles and manage license assignments.
* **`User.Read.All`**: Provides the ability to read all users' full profiles for validation.
* **`email`**: Allows the agent to view the user's email address for identification.
* **`offline_access`**: Ensures the connection can be maintained without constant re-authentication.
* **`User.Read`**: Basic permission to sign in and read the user profile.
* **`User.ReadWrite`**: Allows basic read/write access to individual user profiles.

## **3. Functional Security Scopes**

| Task | Required Permissions |
| --- | --- |
| **Assign Licenses** | `User.ReadWrite.All` |
| **Add User to Security Group** | `Directory.ReadWrite.All` |
| **Create/Update User Profile** | `User.ReadWrite.All`, `Directory.ReadWrite.All` |
| **Verify Group Membership** | `Directory.Read.All`, `User.Read.All` |

## **4. Authentication Flow**

* **Identity:** Microsoft Entra ID App Registration.
* **Secret Management:** The `ClientSecret` is stored securely within Power Automate environment variables or Azure Key Vault.
* **Token Issuance:** The system requests an access token from `https://login.microsoftonline.com/{tenantId}/oauth2/v2.0/token` using the Client Credentials grant.

## **5. Compliance & Auditing**

* **Traceability:** Every action performed (e.g., "User Added to Group") is logged in the Entra ID Audit Logs under the Service Principal's identity.
* **Location Compliance:** The agent enforces the `usageLocation` property on user objects prior to license assignment to satisfy regional licensing requirements.

---
End of Document
