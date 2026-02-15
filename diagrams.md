
### **1. System Context Diagram (Document 01)**

This diagram illustrates the macro-level interaction between the user and the disparate cloud systems.

```mermaid
graph TD
    User((Admin/User)) -->|Natural Language| Teams[Microsoft Teams UI]
    Teams -->|Request| Agent{AI Agent Brain}
    Agent -->|Step 5: Duty Search| MapFlow[Roles Duty Mapping Flow]
    MapFlow -->|Return Role/SKU| Agent
    Agent -->|Step 9: Assign License| LicFlow[License Assignment Flow]
    LicFlow -->|Graph API| M365((Microsoft 365))
    Agent -->|Create Profile| ERP((D365 F&O))

```

---

### **2. Technical Sequence Diagram (Document 02)**

This diagram maps the exact logic found in your `definition.json` for the license assignment process.

```mermaid
sequenceDiagram
    participant Agent as AI Agent
    participant Flow as Power Automate Flow
    participant Graph as Microsoft Graph API

    Agent->>Flow: POST /manual (Email, LicenseInput)
    Note over Flow: Step 9 Logic
    Flow->>Flow: Dictionary Lookup: SKU Part Number
    Flow->>Graph: POST /token (OAuth2 Client Credentials)
    Graph-->>Flow: access_token
    Flow->>Graph: GET /subscribedSkus
    Graph-->>Flow: skuId GUID
    Flow->>Graph: POST /users/{id}/assignLicense
    Graph-->>Flow: 200 OK / Error
    Flow-->>Agent: JSON Response (islicenseassigned)

```

---

### **3. Waterfall Logic Flowchart (Document 03)**

This visualizes the search hierarchy and priority sorting logic used in your mapping flow.

```mermaid
flowchart TD
    Start([Start: Receive Duty Input]) --> Filter[Filter: mserp_skuname != 'NONE']
    Filter --> Duty{Match in DutyName?}
    Duty -- No --> Priv{Match in PrivilegeName?}
    Priv -- No --> Role{Match in RoleName?}
    
    Duty -- Yes --> Sort[Sort by mserp_priority ASC]
    Priv -- Yes --> Sort
    Role -- Yes --> Sort
    Role -- No --> Manual([Flag for Manual Review])
    
    Sort --> Result[Return @first Record]
    Result --> End([End: Handover to Step 8])

```

---

### **Implementation Guide**

1. **Create a new file** in your repository (e.g., `DIAGRAMS.md`).
2. **Paste the code blocks** above into the file.
3. Ensure you wrap them in triple backticks with the `mermaid` identifier like this:
```markdown
```mermaid
[Code goes here]

```


```


```



**Your repository is now technically complete and visually documented.** Would you like me to help you draft a **README.md** that links all these 10 documents and 3 diagrams together into a professional landing page?
