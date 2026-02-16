
### **Document 04: SKU Mapping Reference**

**File Name:** `04_SKU_Mapping_Reference.md`

This document serves as the **Technical Translation Layer**. It ensures that the AI Agent sends the exact string values required by the Power Automate "Dictionary" to avoid provisioning failures.

---

## **1. Purpose**

In Step 9, the Power Automate flow (`Check and Assign License to user in M365`) receives a string from the Agent. It then uses a switch/case or a JSON dictionary to find the corresponding **M365 SkuPartNumber** (e.g., `DYN365_FINANCE_PREMIUM`). If the string does not match exactly, the flow will fail.

---

## **2. Master String Mapping Table**

The Agent must use the **"Exact String Output"** column below when calling the Step 9 flow.

| RECID | SKU Name | Exact String Output (Base) | Exact String Output (Attach) | M365 SkuPartNumber Mapping |
| --- | --- | --- | --- | --- |
| 10 | **SCM Premium** | `Supply Chain Management Premium` | `Supply Chain Management Premium - Attach` | `DYN365_SCM_PREMIUM` |
| 3 | **Finance Premium** | `Finance Premium` | `Finance Premium - Attach` | `DYN365_FINANCE_PREMIUM` |
| 9 | **SCM** | `Supply Chain Management` | `Supply Chain Management - Attach` | `DYN365_SCM_BASE` |
| 2 | **Finance** | `Finance` | `Finance - Attach` | `DYN365_FINANCE_BASE` |
| 1 | **Commerce** | `Commerce` | `Commerce - Attach` | `DYN365_COMMERCE_BASE` |
| 8 | **Project Ops** | `Project Operations` | `Project Operations - Attach` | `DYN365_PROJ_OPS` |
| 4 | **HR** | `Human Resources` | `Human Resources - Attach` | `DYN365_HR` |

---

## **3. String Construction Rules**

To maintain technical integrity during the Step 8 consolidation, the Agent must follow these formatting rules:

1. **Highest Priority SKU:** Output the string exactly as shown in the "Base" column.
* *Example:* `Supply Chain Management Premium`


2. **Secondary SKUs (Attach):** Append a space, a hyphen, and the word "Attach".
* *Format:* `[SKU Name] - Attach`
* *Example:* `Finance Premium - Attach`


3. **Low-Tier SKUs (Suppression):**
* If a Base/Attach license from the table above is present, **do not** send strings for `Team Members`, `Operations - Activity`, or `None`.



---

## **4. Error Handling Mapping**

If the Step 5 flow returns a value not found in this table, the Agent should follow the **Manual Review** protocol defined in Document 02.

| Result | Agent Action |
| --- | --- |
| **Priority 5 (None)** | Do not trigger Step 9. Inform the user: "No billable license required." |
| **Unknown SKU** | Pause execution. Inform the admin: "Unknown SKU detected. Manual assignment required." |

---

## **5. Integration Context**

This mapping directly supports the `D365skupartnumberMapping` variable inside the Power Automate flow `Check and Assign License to user in M365`.

```mermaid
graph LR
    A[Agent Logic] -->|String: 'Finance - Attach'| B[Step 9 Flow]
    B -->|Lookup| C{Dictionary}
    C -->|Match Found| D[SkuId: DYN365_FINANCE_ATTACH]
    D -->|API Call| E[Microsoft Graph]

```

---

**End of Document**
