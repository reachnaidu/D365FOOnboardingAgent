# Automated User Lifecycle Agent

An intelligent agent that orchestrates user onboarding and offboarding across Microsoft Entra ID and Dynamics 365 Finance & Operations (F&O), with automated role mapping, license optimization, and security-first design.

---

## Table of Contents

- [Overview](#overview)
- [Key Features](#key-features)
- [Architecture](#architecture)
- [Workflows](#workflows)
  - [Workflow A: Intelligent Onboarding](#workflow-a-intelligent-onboarding)
  - [Workflow B: Rapid Offboarding](#workflow-b-rapid-offboarding)
- [Technical Implementation](#technical-implementation)
- [Security & Compliance](#security--compliance)
- [Getting Started](#getting-started)
- [Contributing](#contributing)

---

## Overview

This agent automates the complete user lifecycle in enterprise environments by intelligently managing user provisioning, role assignments, and license allocation. It combines natural language processing with rule-based optimization to reduce administrative overhead while maintaining security and cost efficiency.

### Use Cases

- Automated onboarding of new employees with appropriate roles and licenses
- Intelligent license consolidation to minimize costs
- Secure and rapid offboarding of departing users
- Role-based access control (RBAC) enforcement across systems

---

## Key Features

- **Natural Language Processing**: Converts job duty descriptions into D365 role assignments
- **License Optimization**: Applies priority-based rules to eliminate redundant license costs
- **RAG-Enabled Fallback**: Suggests standard roles when exact matches aren't found
- **Validation Engine**: Confirms successful provisioning across all systems before closing cases
- **Security-First Design**: Requires manual confirmation for destructive actions
- **Real-Time Orchestration**: No persistent PII storage

---

## Architecture

The system is built on a four-layer architecture:

```
┌─────────────────────────────────────────┐
│   Interaction Layer (Microsoft Teams)   │
│   • Natural language interface          │
│   • User confirmations                  │
└──────────────┬──────────────────────────┘
               │
┌──────────────▼──────────────────────────┐
│   Orchestration Layer (Agent Logic)     │
│   • Role mapping (Step 5)               │
│   • License consolidation (Step 8)      │
└──────────────┬──────────────────────────┘
               │
┌──────────────▼──────────────────────────┐
│   Integration Layer (Power Automate)    │
│   • Microsoft Graph API gateway         │
│   • License assignment execution        │
└──────────────┬──────────────────────────┘
               │
┌──────────────▼──────────────────────────┐
│   Action Layer (Custom APIs)            │
│   • D365 F&O user provisioning          │
│   • Direct database interactions        │
└─────────────────────────────────────────┘
```

---

## Workflows

### Workflow A: Intelligent Onboarding

#### Step 5: Role Determination

**Input**: Natural language job duties (e.g., "manage accounts payable and inventory")

**Process**:
1. Parse duties into structured array
2. Map duties to D365 `RoleNames`
3. If no match found → Query RAG-based Knowledge Base for suggestions

#### Step 8: License Consolidation

**Optimization Strategy**: Priority-based suppression to reduce costs

**Priority Hierarchy**:
- Finance: 90
- Supply Chain Management: 80
- Commerce, Human Resources, Project Operations: (lower priorities)

**Logic**:
- If a higher-priority license covers the same capabilities, suppress lower-priority licenses
- Append appropriate suffixes: `- base` or `- attach`

**Example**:
```
Input: User needs Finance and SCM capabilities
Output: Finance - base (SCM suppressed as Finance has higher priority)
```

#### Step 9: Technical Execution

**Power Automate Flow**: "Check and Assign License to user in M365"

**Required Payload**:
```json
{
  "email": "user@company.com",
  "LicenseInput": "Finance / Supply Chain Management - Attach"
}
```

**Valid License Keys**:
- `Finance` / `Finance - Attach`
- `Supply Chain Management` / `Supply Chain Management - Attach`
- `Commerce`
- `Human Resources`
- `Project Operations`

---

### Workflow B: Rapid Offboarding

**Security Requirement**: Manual confirmation required before execution

**Process**:
1. Request confirmation from administrator
2. Call custom API with user email
3. Toggle user status to "Disabled" in D365 F&O
4. Revoke access across integrated systems

---

## Technical Implementation

### Step 10: Operational Validation

Before closing a provisioning case, the agent validates success across all systems:

| System | Validation Criteria |
|--------|-------------------|
| **Azure AD** | Object ID must be active |
| **D365 User** | Successfully mapped to Legal Entity |
| **Licenses** | `islicenseassigned: true` in Power Automate response |

**Failure Handling**: If any validation fails, the agent reports the specific issue and suggests remediation steps.

---

## Security & Compliance

### Authentication
- **Service Principal**: Power Automate uses service principal authentication for Microsoft Graph API access
- **Least Privilege Principle**: Agent has minimal permissions required for operations

### Data Protection
- **No Persistent Storage**: PII is never stored; agent operates as real-time orchestrator only
- **Audit Trail**: All actions logged for compliance review
- **Manual Approvals**: Destructive operations (offboarding) require explicit confirmation

### Compliance Standards
- GDPR-compliant data handling
- SOC 2 Type II aligned security controls
- Zero Trust architecture principles

---

## Getting Started

### Prerequisites

- Microsoft Teams workspace with agent integration enabled
- Power Automate Premium license
- Dynamics 365 F&O instance with API access
- Microsoft Entra ID (Azure AD) administrative privileges

### Configuration

1. **Deploy Power Automate Flows**:
   - Import "Check and Assign License to user in M365" flow
   - Configure service principal credentials

2. **Set Up Custom APIs**:
   - Deploy D365 F&O user provisioning endpoints
   - Configure authentication tokens

3. **Configure Agent**:
   - Update role mapping database
   - Train RAG Knowledge Base with organizational role definitions
   - Set license priority hierarchy

4. **Test Workflows**:
   ```
   Test Onboarding: Create dummy user with sample job duties
   Test Offboarding: Deactivate test account
   Validate: Confirm all systems reflect changes
   ```

---

## Contributing

We welcome contributions! Please follow these guidelines:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### Development Standards

- Follow existing code style and naming conventions
- Add unit tests for new features
- Update documentation for API changes
- Ensure security best practices are maintained

---

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## Support

For issues, questions, or feature requests:
- **Email**: support@company.com
- **Documentation**: [Internal Wiki](https://wiki.company.com/user-lifecycle-agent)
- **Issue Tracker**: [GitHub Issues](https://github.com/company/user-lifecycle-agent/issues)

---

## Changelog

### Version 1.0.0 (Current)
- Initial release with dual-track onboarding/offboarding workflows
- License optimization engine
- RAG-based role suggestion system
- Validation engine for provisioning verification

---

**Maintained by**: IT Infrastructure Team  
**Last Updated**: February 2026
