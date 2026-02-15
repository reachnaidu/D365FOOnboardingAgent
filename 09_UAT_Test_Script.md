**File Name:** `09_UAT_Test_Script.md`

This document provides the test cases necessary to validate that the Agent is correctly interpreting the **Priority Rule** (SCM 100 > Finance 90) and correctly communicating with the Power Automate flows.

---

## **1. Testing Environment**

* **Platform:** Microsoft Teams (AI Agent Interface).
* **Backend:** D365 F&O Sandbox & M365 Dev Tenant.
* **Required Data:** Test User account (e.g., `uat_tester@yourdomain.com`).

## **2. Test Case Matrix**

| ID | Scenario | Input Duty | Expected Base SKU | Expected Attach SKU | Status |
| --- | --- | --- | --- | --- | --- |
| **UAT-01** | **Single High-Tier** | "Manage Warehouse" | SCM Premium (100) | None | [ ] |
| **UAT-02** | **Multi-Tier Logic** | "Warehouse & Budget" | SCM Premium (100) | Finance Premium - Attach | [ ] |
| **UAT-03** | **Priority Flip Test** | "Invoicing & SCM" | SCM (80) | Finance - Attach | [ ] |
| **UAT-04** | **Low-Tier Suppression** | "SCM & Team Member" | SCM (80) | (Team Member Suppressed) | [ ] |
| **UAT-05** | **None Filter** | "Guest View" | (Manual Review Flag) | N/A | [ ] |

---

## **3. Detailed Step-by-Step Execution (Example: UAT-02)**

1. **Initiate:** Type "Onboard a new user" to the Agent.
2. **Identity:** Provide the name `Test User` and email `test@domain.com`.
3. **Input:** When asked for duties, enter: *"They need to manage premium warehouse operations and handle premium financial reporting."*
4. **Verification (Step 5):** Observe if the Agent calls the mapping flow.
5. **Logic Check (Step 8):** Verify the Agent says: *"I have identified SCM Premium as the Base and Finance Premium as an Attach license."*
6. **Provisioning (Step 9):** Approve the request.
7. **Success Confirmation:** Ensure the Agent returns: *"License assigned successfully. User added to Security Group: D365_SCM_PREMIUM."*

---

## **4. Failure Recovery Testing**

* **Goal:** Ensure the Agent doesn't "hallucinate" a success if the API fails.
* **Action:** Temporarily revoke the Service Principal's permissions in Entra ID.
* **Expected Result:** Agent must report: *"Error: Insufficient permissions to assign license. Case flagged for manual IT intervention."*

---
END OF DOCUMENT

---
