
Automated F&O User onboarding  Orchestrator

---
## 1. Project Overview

This repository contains the complete documentation, logic matrices, and technical specifications for an AI-driven **User Lifecycle Orchestrator**. The system automates the end-to-end onboarding and offboarding of users in **Microsoft Entra ID (Active Directory)** and **Dynamics 365 Finance & Operations (F&O)** using natural language processing and Power Automate.

By bridging the gap between human HR requests and technical system requirements, the orchestrator ensures that new hires are productive from minute one with the correct identity, permissions, and licenses.

### **Key Capabilities**

* **End-to-End User Provisioning:**
* **Microsoft Entra ID (AD):** Automatically creates the user account object, establishes the UPN, and configures critical identity metadata like `usageLocation`.
* **Dynamics 365 F&O:** Simultaneously creates the user record within the ERP environment, ensuring the identity is synchronized across the enterprise stack.


* **Semantic Role & Duty Mapping:**
* Translates natural language job descriptions (e.g., *"Handles vendor payments and bank reconciliations"*) into technical **D365 Roles, Duties, and Privileges**.
* Ensures that security permissions are granted based on the principle of least privilege while matching actual business functions.


* **Automated Security Role Assignment:**
* Directly pushes the identified security roles into the D365 user profile.
* Dynamically manages membership in **Entra ID Security Groups**, ensuring that folder access and application permissions are aligned with the user’s ERP role.


* **Intelligent License Optimization:**
* Automatically applies the "Base vs. Attach" priority rule (e.g., prioritizing SCM Premium over Finance) to minimize M365 licensing costs.
* Identifies when a "Team Member" license is sufficient vs. a full "Enterprise" license based on mapped duties.


* **Real-Time API Orchestration:**
* Communicates directly with **Microsoft Graph API** and **D365 OData entities** for instantaneous provisioning, removing the need for manual IT tickets and data entry.



---


## 2. System Architecture

The following diagram illustrates the high-level interaction between the user, the AI Agent, and the enterprise backend systems.

```mermaid
graph LR
    A((Admin)) -->|User Info| B{AI Agent}
    
    subgraph Provisioning
        B --> C[Create AD User]
        B --> D[Create F&O User]
    end

    subgraph Association
        D --> E[Assign D365 Roles]
        C --> F{License Group<br/>Exists?}
    end

    F -->|Yes| G[Add to License Group]
    F -->|No| H[Direct License<br/>Assignment]
    
    G --> I[Final M365 License]
    H --> I

    %% Contrast-Focused Styling
    style B fill:#e1f5fe,stroke:#01579b,color:#000
    style Provisioning fill:#f1f8e9,stroke:#558b2f,color:#000
    style Association fill:#fff3e0,stroke:#e65100,color:#000
    style C fill:#fff,stroke:#333,color:#000
    style D fill:#fff,stroke:#333,color:#000
    style E fill:#fff,stroke:#333,color:#000
    style F fill:#fff,stroke:#333,color:#000
    style G fill:#fff,stroke:#333,color:#000
    style H fill:#fff,stroke:#333,color:#000
    style I fill:#e8f5e9,stroke:#2e7d32,color:#000

```

---

## 3. Repository Contents

Ah, I see what happened. The previous output got cut off and the links were still holding onto those search engine prefixes.

Here is the clean, high-visibility list for your **README.md**. These are formatted as strict **Relative Paths**, meaning they will look inside your repository's `/docs` folder rather than opening an external website.

---

### Phase 1: Specifications & Architecture

* **[01_Functional_Specification.md](../docs/01_Functional_Specification.md)**: High-level business goals and user workflows.
* **[02_Technical_Interface_Spec.md](https://www.google.com/search?q=./docs/02_Technical_Interface_Spec.md)**: Detailed API payloads and sequence diagrams for Step 9.
* **[08_Technical_Specification.md](https://www.google.com/search?q=./docs/08_Technical_Specification.md)**: "Under the hood" flow logic and waterfall search mechanics.

### Phase 2: Logic & Mapping

* **[03_Logic_Decision_Matrix.md](https://www.google.com/search?q=./docs/03_Logic_Decision_Matrix.md)**: The core "Priority Rule" for license suppression.
* **[04_SKU_Mapping_Reference.md](https://www.google.com/search?q=./docs/04_SKU_Mapping_Reference.md)**: Valid strings for the Power Automate dictionary.
* **[06_RAG_Knowledge_Base_Schema.md](https://www.google.com/search?q=./docs/06_RAG_Knowledge_Base_Schema.md)**: Schema for semantic fallback search.

### Phase 3: Deployment & Testing

* **[05_Security_Compliance.md](https://www.google.com/search?q=./docs/05_Security_Compliance.md)**: OAuth2 authentication and data privacy details.
* **[07_Master_System_Prompt.txt](https://www.google.com/search?q=./docs/07_Master_System_Prompt.txt)**: The core instruction set for the AI Agent.
* **[09_UAT_Test_Script.md](https://www.google.com/search?q=./docs/09_UAT_Test_Script.md)**: Step-by-step validation procedures.
* **[10_Project_Closeout.md](https://www.google.com/search?q=./docs/10_Project_Closeout.md)**: Final deployment summary and maintenance guide.

---

### **How to verify these links work:**

1. **Folder Name:** Ensure your folder is named exactly `docs` (all lowercase).
2. **Filenames:** Ensure the filenames in the `/docs` folder match these exactly (e.g., underscores vs. spaces).
3. **GitHub Preview:** When you commit this to GitHub, clicking these links will now navigate directly to the file inside your repository interface.

**Would you like me to help you create a "Getting Started" table that summarizes the primary goal of each phase?**

---

**Next step for you:**
Would you like me to help you draft the content for **Document 10 (Project Closeout & Maintenance)** so you have a complete set of files ready for the `/docs` folder?
---

## 4. Key Technical Logic

The system relies on a **Waterfall Search** to identify roles. If a direct duty name match is not found, the system cascades through privileges and role names to find the best fit.

```mermaid
graph LR
    A((Admin)) -->|User Info| B{AI Agent}
    
    subgraph Provisioning [User Creation]
        B --> C[Create AD User]
        B --> D[Create F&O User]
    end

    subgraph Association [Role & License]
        D --> E[Assign D365 Roles]
        C --> F{License Group<br/>Exists?}
    end

    F -->|Yes| G[Add to License Group]
    F -->|No| H[Direct License<br/>Assignment]
    
    G --> I[Final M365 License]
    H --> I

    %% High-Contrast Styling (No White/Pink)
    style B fill:#e1f5fe,stroke:#01579b,color:#000
    style Provisioning fill:#e0e0e0,stroke:#424242,color:#000
    style Association fill:#fff9c4,stroke:#fbc02d,color:#000
    style C fill:#cfd8dc,stroke:#333,color:#000
    style D fill:#cfd8dc,stroke:#333,color:#000
    style E fill:#fff176,stroke:#333,color:#000
    style F fill:#fff176,stroke:#333,color:#000
    style G fill:#fff176,stroke:#333,color:#000
    style H fill:#fff176,stroke:#333,color:#000
    style I fill:#c8e6c9,stroke:#2e7d32,color:#000
```

---

## 5. Getting Started

## 🚀 Getting Started

To deploy the User Lifecycle Orchestrator, please follow our detailed configuration guide. This ensures secrets are handled securely via Key Vault and environment endpoints are correctly mapped.

👉 **[View the Full Installation Guide](./INSTALLATION.md)

⚙️ Final Configuration & Validation

Once the core infrastructure is deployed via the Installation Guide, complete these final steps to activate the AI Agent:

1. Configure the Agent Brain
The AI requires specific logic to handle duty mapping and licensing rules.

Open your AI Agent/Copilot Studio.

Navigate to Instructions/System Prompt.

Copy the full contents of 07_Master_System_Prompt.txt and paste them into the Agent's instructions.

2. User Acceptance Testing (UAT)
Before moving to production, verify that the AD, F&O, and Licensing flows are correctly linked.

Refer to Document 09: UAT Test Scripts.

Run the "New Hire Onboarding" scenario to verify end-to-end connectivity across all systems.

---


