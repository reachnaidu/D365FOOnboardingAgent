# Document 01: Functional Specification - F&O User onboarding Agent

## 1. Executive Summary

This agent is designed to automate the end-to-end lifecycle of users within the enterprise ecosystem, specifically targeting **Microsoft Entra ID (Azure AD)** and **Dynamics 365 Finance & Operations (F&O)**. The primary goal is to reduce manual IT tickets by translating job duties into technical permissions.

## 2. Business Objectives

* **Automation:** Eliminate manual data entry for user profile creation.
* **Accuracy:** Use a "Waterfall Search" to ensure job duties map to the most relevant D365 roles.
* **Cost Governance:** Minimize license waste by applying optimization logic before assignment.

## 3. Workflow A: Intelligent Onboarding

The onboarding process follows a 10-step sequence:

1. **Identity Capture:** Agent collects User Name, Email, and Legal Entity.
2. **Job Duty Analysis:** The user provides natural language duties (e.g., "I process invoices").
3. **Role Mapping (Step 5):** The agent calls a Power Automate flow to search for matching D365 roles.
* The flow searches Duty Name, then Privilege Name, then Role Name.
* Roles marked with "NONE" SKU are filtered out to ensure only billable roles are considered.


4. **License Consolidation (Step 8):** If multiple roles are found, the agent applies the **Priority Rule**.
* The highest priority role becomes the "Base" license.
* Secondary roles are assigned as "Attach" licenses to reduce costs.


5. **Provisioning (Step 9):** The agent triggers the M365 license assignment via Microsoft Graph.
* User's `usageLocation` is automatically set to "US".


6. **Validation (Step 10):** The agent confirms success or flags for manual review based on the flow's `islicenseassigned` status.

## 4. Workflow B: Rapid Offboarding

* **Trigger:** An admin requests a user deactivation.
* **Security:** The agent requires a "Confirm Deactivation" step.
* **Action:** Calls a custom API to disable the user in D365 F&O and notifies IT to revoke M365 access.

## 5. System Architecture Overview

---

### **Next Step for you:**

Once you have pasted this into **01_Functional_Specification.md**, would you like me to provide the specific data for **02_Technical_Interface_Spec.md**, which focuses on the API inputs/outputs like `flowinputjobduty` and `LicenseInput`?
