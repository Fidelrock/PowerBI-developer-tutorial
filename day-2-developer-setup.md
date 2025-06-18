# Day 2: Developer Environment Setup - Interactive Session

## 🎯 **What You'll Learn Today:**
- Setting up Azure Active Directory (AAD)
- Creating Power BI service accounts
- Configuring API permissions
- Setting up your development workspace

---

## **Step 1: Azure Active Directory (AAD) Setup**

### **🔍 Why AAD?**
Azure Active Directory is Microsoft's **identity and access management** service. For Power BI development, you need it for:
- API authentication
- User management
- Security and permissions

### **🎯 Interactive Exercise: AAD Setup**

**Option A: Using Existing Microsoft Account**
1. Go to https://portal.azure.com
2. Sign in with your Microsoft account
3. Search for "Azure Active Directory"
4. Click on "Azure Active Directory"

**Option B: Creating New AAD Tenant**
1. Go to https://portal.azure.com
2. Click "Create a resource"
3. Search for "Azure Active Directory"
4. Click "Create"
5. Fill in:
   - **Organization name**: "My Power BI Dev"
   - **Initial domain name**: "mypowerbidev.onmicrosoft.com"
   - **Country/Region**: Your location

**💡 Pro Tip:** If you have a Microsoft 365 account, you already have an AAD tenant!

---

## **Step 2: Power BI Service Account Creation**

### **🔍 What is a Service Account?**
A service account is a **non-human account** used for:
- API authentication
- Automated processes
- Application integration

### **🎯 Interactive Exercise: Create Service Account**

**Step 2.1: Create Application Registration**
1. In Azure Portal, go to "Azure Active Directory"
2. Click "App registrations" → "New registration"
3. Fill in:
   - **Name**: "Power BI Developer App"
   - **Supported account types**: "Accounts in this organizational directory only"
   - **Redirect URI**: Leave blank for now
4. Click "Register"

**Step 2.2: Get Your Credentials**
1. After registration, note down:
   - **Application (client) ID** - You'll need this for API calls
   - **Directory (tenant) ID** - Your AAD tenant ID
2. Go to "Certificates & secrets"
3. Click "New client secret"
4. Add description: "Power BI API Access"
5. Set expiration: "24 months"
6. **IMPORTANT**: Copy the secret value immediately (you won't see it again!)

**💡 Security Note:** Store these credentials securely - never commit them to code!

---

## **Step 3: API Permissions Configuration**

### **🔍 What Permissions Do You Need?**
For Power BI development, you typically need:
- **Dataset permissions** - Read/write datasets
- **Report permissions** - Read/write reports
- **Workspace permissions** - Manage workspaces
- **User permissions** - Read user information

### **🎯 Interactive Exercise: Configure Permissions**

**Step 3.1: Add Power BI Permissions**
1. In your app registration, go to "API permissions"
2. Click "Add a permission"
3. Select "Power BI Service"
4. Choose "Delegated permissions"
5. Select these permissions:
   - ✅ Dataset.ReadWrite.All
   - ✅ Report.ReadWrite.All
   - ✅ Workspace.ReadWrite.All
   - ✅ User.Read.All

**Step 3.2: Grant Admin Consent**
1. Click "Grant admin consent for [Your Organization]"
2. Confirm the permissions

**💡 What this means:** Your app can now perform these operations on behalf of users

---

## **Step 4: Development Workspace Configuration**

### **🔍 Why a Dedicated Development Workspace?**
- **Isolation** - Keep development separate from production
- **Testing** - Safe environment for experiments
- **Collaboration** - Share with development team
- **Security** - Control access to development resources

### **🎯 Interactive Exercise: Setup Development Workspace**

**Step 4.1: Create Development Workspace**
1. Go to https://app.powerbi.com
2. Click "Workspaces" → "Create a workspace"
3. Fill in:
   - **Workspace name**: "Power BI Dev Environment"
   - **Description**: "Development workspace for Power BI projects"
   - **Privacy**: "Public" (for development)
4. Click "Save"

**Step 4.2: Configure Workspace Settings**
1. In your workspace, click "Settings" (gear icon)
2. Go to "Workspace access"
3. Add your development team members
4. Set appropriate roles:
   - **Admin**: Full control
   - **Member**: Can edit content
   - **Contributor**: Can add content
   - **Viewer**: Read-only access

**Step 4.3: Create Sample Content**
1. Create a simple dataset (use the sample data from Day 1)
2. Create a basic report
3. Create a dashboard
4. Test the workspace functionality

---

## **Step 5: Environment Variables Setup**

### **🔍 Why Environment Variables?**
- **Security** - Keep credentials out of code
- **Flexibility** - Easy to switch between environments
- **Best Practice** - Industry standard for configuration

### **🎯 Interactive Exercise: Setup Environment Variables**

**Step 5.1: Create Configuration File**
Create a file called `.env` (we'll use this later):

```bash
# Power BI API Configuration
POWERBI_TENANT_ID=your_tenant_id_here
POWERBI_CLIENT_ID=your_client_id_here
POWERBI_CLIENT_SECRET=your_client_secret_here
POWERBI_WORKSPACE_ID=your_workspace_id_here

# Development Settings
POWERBI_API_URL=https://api.powerbi.com/v1.0/myorg
POWERBI_AUTHORITY=https://login.microsoftonline.com
```

**Step 5.2: Get Workspace ID**
1. In your Power BI workspace, go to "Settings"
2. Copy the "Workspace ID" (GUID format)

**💡 Security Reminder:** Never commit `.env` files to version control!

---

## **Step 6: Testing Your Setup**

### **🎯 Interactive Exercise: Test Your Configuration**

**Step 6.1: Test API Connection**
We'll create a simple test script to verify everything works:

```javascript
// test-connection.js (we'll create this in the next session)
const { PowerBIClient } = require('@azure/ms-rest-azure-js');
const { TokenCredentials } = require('@azure/ms-rest-js');

// Test basic connectivity
async function testConnection() {
    // We'll implement this in the next session
    console.log("Testing Power BI API connection...");
}
```

**Step 6.2: Verify Permissions**
1. Try to list workspaces via API
2. Try to create a test dataset
3. Verify workspace access

---

## **🎯 Day 2 Summary & Checklist**

### **✅ What You Should Have Completed:**
- [ ] Azure Active Directory tenant (or verified existing)
- [ ] Application registration created
- [ ] Client ID and secret obtained
- [ ] API permissions configured
- [ ] Development workspace created
- [ ] Environment variables configured
- [ ] Basic connectivity tested

### **🔍 Troubleshooting Common Issues:**

**Issue: "Insufficient permissions"**
- Solution: Check if admin consent was granted
- Verify the user has appropriate Power BI licenses

**Issue: "Invalid client secret"**
- Solution: Generate a new client secret
- Ensure you copied the value immediately after creation

**Issue: "Workspace not found"**
- Solution: Verify workspace ID is correct
- Check if you have access to the workspace

### **🎯 Hands-on Challenge:**
Create a complete development environment:
1. Set up AAD with proper permissions
2. Create a service account
3. Configure a development workspace
4. Test API connectivity
5. Document your setup process

### **📚 Additional Resources:**
- [Azure AD App Registration Guide](https://docs.microsoft.com/en-us/azure/active-directory/develop/quickstart-register-app)
- [Power BI API Permissions](https://docs.microsoft.com/en-us/power-bi/developer/embedded/power-bi-embedded-app-token-flow)
- [Workspace Management](https://docs.microsoft.com/en-us/power-bi/collaborate-share/service-create-workspaces)

**Next: Day 3 - Power BI REST API Basics** 
