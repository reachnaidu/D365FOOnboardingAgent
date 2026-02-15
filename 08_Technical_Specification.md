# Document 08: Technical Specification

## 1. Scope

This specification covers the technical implementation of the **Step 5: Role & Duty Mapping** logic. It defines how the system queries the `mserp_licensingrolemappingentities` table and handles result prioritization.

## 2. Data Source

* **Entity Name:** `mserp_licensingrolemappingentities`
* **Source System:** Dynamics 365 Finance & Operations (via Dataverse).
* **Key Attributes:**
* `mserp_dutyname`: Human-readable duty.
* `mserp_privilegename`: Granular security privilege.
* `mserp_rolename`: The target ERP Security Role.
* `mserp_skuname`: The required license part name.
* `mserp_priority`: The numerical value (10–100) used for optimization.



## 3. The Waterfall Search Algorithm

To ensure the AI Agent finds the correct role even with vague user input, the flow executes a "cascading" search. If a search stage returns one or more records, the flow stops and moves to **Sort & Filter**.

### Stage 1: Duty Filter

```sql
SELECT * FROM mserp_licensingrolemappingentities 
WHERE mserp_dutyname CONTAINS 'InputText' 
AND mserp_skuname != 'None'

```

### Stage 2: Privilege Fallback

*If Stage 1 is empty:*

```sql
SELECT * FROM mserp_licensingrolemappingentities 
WHERE mserp_privilegename CONTAINS 'InputText' 
AND mserp_skuname != 'None'

```

### Stage 3: Role Fallback

*If Stage 2 is empty:*

```sql
SELECT * FROM mserp_licensingrolemappingentities 
WHERE mserp_rolename CONTAINS 'InputText' 
AND mserp_skuname != 'None'

```

## 4. Sorting & Final Selection

Once matches are found, the system must handle "Many-to-One" scenarios (where one duty might map to multiple SKU possibilities).

1. **Collect** all matches from the successful Stage.
2. **Sort** the array by `mserp_priority` in **Ascending** order.
3. **Return** only the `@first()` record to the AI Agent.

## 5. Error Handling & Edge Cases

| Scenario | Logic Action |
| --- | --- |
| **No Match Found** | Return a "Null" response. Agent triggers the RAG Knowledge Base fallback. |
| **Priority 5 (None)** | Records are filtered out at the query level to prevent non-billable roles from being suggested. |
| **Multiple Groups** | If the search returns roles in different groups, the Agent (in Step 8) will treat the highest priority as "Base" and the other as "Attach." |

---

END OF DOCUMENT
