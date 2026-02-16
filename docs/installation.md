# Installation & Configuration Guide

Follow these steps in order to ensure the AI Agent has the necessary permissions to provision users and assign licenses.

---

## 🛠 Step 1: Microsoft Entra ID Setup (App Registration)
The system uses an App Registration to communicate with the MS Graph API.

1.  Navigate to **Microsoft Entra admin center** > **App registrations**.
2.  Create a **New registration** (e.g., `AI-User-Orchestrator`).
3.  Under **API Permissions**, add the following **Application Permissions**:
    * `User.ReadWrite.All`
    * `Directory.ReadWrite.All`
    * `Organization.Read.All`
4.  Grant **Admin Consent** for the tenant.

## 🔐 Step 2: Secure Secrets in Azure Key Vault
*Sensitive credentials must be stored as Secrets.*

1.  Navigate to your **Azure Key Vault**.
2.  Create the following **Secrets**:
    * `client-id`: The Application ID from Step 1.
    * `client-secret`: The Secret value generated in Step 1.
3.  Ensure the Power Automate Service Principal has **Secret User** access to this Key Vault.



## 🏗 Step 3: D365 Finance & Operations Setup
1.  **Import Packages:** Deploy the custom F&O models provided in the `/packages` folder.
2.  **Authorize App:** In F&O, go to `System Administration > Setup > Microsoft Entra ID applications`. Add the **Client-ID** to authorize the orchestrator to create ERP users.

## ⚡ Step 4: Power Platform Deployment & Environment Variables
1.  **Import Solution:** Import the `UserOrchestrator_managed.zip`.
2.  **Update Connections:** Authenticate the connection references for Dataverse and MS Graph.
3.  **Configure Environment Variables:** Set the following non-sensitive endpoints:
    * `TenantID`: Your Microsoft Entra Tenant ID.
    * `FO_URL`: The base URL for your D365 environment (e.g., `https://your-env.operations.dynamics.com`).

---

## ✅ Deployment Checklist
| Task | Storage Location |
| :--- | :--- |
| **App Client ID** | Azure Key Vault (Secret) |
| **App Client Secret** | Azure Key Vault (Secret) |
| **Tenant ID** | Environment Variable |
| **F&O Environment URL** | Environment Variable |
