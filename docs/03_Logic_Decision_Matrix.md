
# Document 03: Logic & Decision Matrix

## 1. Overview

This document defines the automated reasoning used by the Agent to map job duties to technical roles and optimize license costs. The logic is divided into **Discovery (Waterfall Search)** and **Optimization (Priority & Grouping Rules)**.

## 2. Waterfall Search Logic (Discovery Phase)

To ensure high accuracy, the system does not rely on a single keyword match. It executes a cascading search within the `mserp_licensingrolemappingentities` table.

### 2.1 The Search Hierarchy

The Agent processes the user's input through the following stages until a match is found:

1. **Filter:** Exclude all records where `SKUNAME` is "None" or `PRIORITY`  5.
2. **Stage 1 (Duty):** Search for the input within the `mserp_dutyname` field.
3. **Stage 2 (Privilege):** If Stage 1 is empty, search within `mserp_privilegename`.
4. **Stage 3 (Role):** If Stage 2 is empty, search within `mserp_rolename`.

### 2.2 Result Selection

If multiple records match at any stage, the flow sorts them by **Priority (Descending)** and selects the `@first()` record.

---

## 3. Priority & Grouping Rules (Optimization Phase)

Once a list of required roles is identified, the Agent applies the following matrix to determine the "Base" license and identify "Attach" opportunities.

### 3.1 Master Priority Matrix

| RECID | SKU Name | Priority | Group Name |
| --- | --- | --- | --- |
| 10 | **Supply Chain Management Premium** | **100** | Base - Commerce, Finance, SCM |
| 3 | **Finance Premium** | **90** | Base - Commerce, Finance, SCM |
| 9 | **Supply Chain Management** | **80** | Base - Commerce, Finance, SCM |
| 2 | **Finance** | **70** | Base - Commerce, Finance, SCM |
| 1 | **Commerce** | **60** | Base - Commerce, Finance, SCM |
| 8 | **Project Operations** | **50** | Base - HR, Project Operations |
| 4 | **Human Resources** | **40** | Base - HR, Project Operations |
| 7 | Operations - Activity | 30 | N/A |
| 11 | Team Members | 20 | N/A |
| 5 | HR Self Service | 10 | N/A |
| 6 | None | 5 | N/A |

### 3.2 Consolidation Logic (The "Base vs. Attach" Rule)

The Agent optimizes costs by grouping related licenses:

1. **Group Identification:** The Agent looks at the `GROUPNAME` for all identified SKUs.
2. **Base Assignment:** The SKU with the highest absolute `PRIORITY` score is designated as the **Base** license.
3. **Attach Assignment:** * Any other SKU within the **same Group Name** is automatically converted to an **Attach** SKU.
* Any SKU in a **different Group Name** that has a Priority  40 is also converted to an **Attach** SKU.


4. **Low-Tier Suppression:** * If any license with Priority  40 is present, the Agent **discards** requests for Priority 10, 20, or 30 licenses (Activity/Team Members), as their permissions are inherited by the higher-tier licenses.

---

## 4. Logical Example

**Input Duties:** "Manage Premium Warehouse" and "Process Premium Invoices."

* **Step 5 Results:** 1. `Supply Chain Management Premium` (Pri: 100, Group: SCM/Fin/Comm)
2. `Finance Premium` (Pri: 90, Group: SCM/Fin/Comm)
* **Step 8 Decision:**
* **Base:** `Supply Chain Management Premium` (Highest Priority).
* **Attach:** `Finance Premium - Attach` (Same Group, lower priority).


* **Final Output to Step 9:** `["Supply Chain Management Premium", "Finance Premium - Attach"]`

---

**End of Document**
