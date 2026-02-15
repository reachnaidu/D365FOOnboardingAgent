CREATE KEY VAULT FOR STORING SECRET AND VALUE 
<img width="783" height="852" alt="image" src="https://github.com/user-attachments/assets/444c1c79-fb27-4550-b349-5d5bf0bac0c8" />
PROVIDE NECESSARY ROLES FOR THE KEY VAAULT TO CREATE client-id and client-secret(Store secret value geenerated from App Registration
<img width="1074" height="353" alt="image" src="https://github.com/user-attachments/assets/47b30d58-b28a-4bd4-b180-31314bba8b22" />

<img width="1242" height="439" alt="image" src="https://github.com/user-attachments/assets/4af5cdd0-f6e5-4b58-bb7f-a9fdd2f93604" />

Grant access to Power Automate
You must grant Key Vault access to the identity that Power Automate uses.
Choose ONE of these:
✅ Option 1: User-based connection

Grant Get permission to the user account creating the Key Vault connection

✅ Option 2: Service Principal (best practice)

Register an Azure AD App
Grant Key Vault Secret → Get
Use that app when creating the connector connection


Key Vault access policies explicitly control secret access

CREATE CONNECTION FROM POWER AUTOMATE TO AZURE KEY VAULT
<img width="787" height="412" alt="image" src="https://github.com/user-attachments/assets/b1197f90-bc4d-4a77-9f7e-94ceb3cb7dbb" />
Create Azure Key Vault connection in Power Automate
In Power Automate → Data → Connections:

New connection
Choose Azure Key Vault
Authenticate (user or app)
