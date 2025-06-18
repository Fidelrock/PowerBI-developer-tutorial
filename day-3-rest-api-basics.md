# Day 3: Power BI REST API Basics - Interactive Session

## 🎯 **What You'll Learn Today:**
- Understanding REST API fundamentals
- Power BI REST API endpoints
- Authentication with Azure AD
- Making your first API calls
- Error handling and best practices

---

## **Step 1: REST API Fundamentals**

### **🔍 What is a REST API?**
REST (Representational State Transfer) is an **architectural style** for building web services.

**Think of it like this:**
- **GET** = Read data (like opening a book)
- **POST** = Create new data (like writing a new page)
- **PUT** = Update existing data (like editing a page)
- **DELETE** = Remove data (like tearing out a page)

### **🎯 Interactive Exercise: Understanding HTTP Methods**

Let's practice with a simple example:

```javascript
// GET - Read workspaces
GET https://api.powerbi.com/v1.0/myorg/workspaces

// POST - Create a new workspace
POST https://api.powerbi.com/v1.0/myorg/workspaces
{
  "name": "My New Workspace"
}

// PUT - Update workspace settings
PUT https://api.powerbi.com/v1.0/myorg/workspaces/{workspaceId}
{
  "name": "Updated Workspace Name"
}

// DELETE - Remove workspace
DELETE https://api.powerbi.com/v1.0/myorg/workspaces/{workspaceId}
```

---

## **Step 2: Power BI REST API Overview**

### **🔍 API Base URL**
All Power BI API calls start with: `https://api.powerbi.com/v1.0/myorg/`

### **🎯 Interactive Exercise: Explore API Endpoints**

**Step 2.1: Main Resource Endpoints**
Here are the key endpoints you'll use:

```javascript
// Workspaces
GET /workspaces                    // List all workspaces
GET /workspaces/{id}              // Get specific workspace
POST /workspaces                  // Create workspace
DELETE /workspaces/{id}           // Delete workspace

// Datasets
GET /workspaces/{id}/datasets     // List datasets in workspace
GET /datasets/{id}                // Get specific dataset
POST /workspaces/{id}/datasets    // Create dataset
DELETE /datasets/{id}             // Delete dataset

// Reports
GET /workspaces/{id}/reports      // List reports in workspace
GET /reports/{id}                 // Get specific report
POST /workspaces/{id}/reports     // Create report
DELETE /reports/{id}              // Delete report

// Dashboards
GET /workspaces/{id}/dashboards   // List dashboards
GET /dashboards/{id}              // Get specific dashboard
POST /workspaces/{id}/dashboards  // Create dashboard
DELETE /dashboards/{id}           // Delete dashboard
```

**Step 2.2: Interactive API Explorer**
1. Go to [Power BI REST API Reference](https://learn.microsoft.com/en-us/rest/api/power-bi/)
2. Click on "Workspaces" → "Get Workspaces"
3. Try the "Try it" feature (if available)
4. Note the response format

---

## **Step 3: Authentication with Azure AD**

### **🔍 How Authentication Works**
Power BI uses **OAuth 2.0** with Azure AD for authentication.

**The Flow:**
1. Your app requests an access token from Azure AD
2. Azure AD validates your credentials
3. Azure AD returns an access token
4. You include the token in API requests

### **🎯 Interactive Exercise: Get Access Token**

**Step 3.1: Using Postman (Recommended for Testing)**

1. Download [Postman](https://www.postman.com/downloads/)
2. Create a new request
3. Set up OAuth 2.0 authentication:

```javascript
// OAuth 2.0 Settings
Grant Type: Client Credentials
Access Token URL: https://login.microsoftonline.com/{tenant-id}/oauth2/v2.0/token
Client ID: {your-client-id}
Client Secret: {your-client-secret}
Scope: https://analysis.windows.net/powerbi/api/.default
```

**Step 3.2: Using JavaScript (Node.js)**

Create a file called `auth-test.js`:

```javascript
const axios = require('axios');
require('dotenv').config();

async function getAccessToken() {
    try {
        const response = await axios.post(
            `https://login.microsoftonline.com/${process.env.POWERBI_TENANT_ID}/oauth2/v2.0/token`,
            new URLSearchParams({
                grant_type: 'client_credentials',
                client_id: process.env.POWERBI_CLIENT_ID,
                client_secret: process.env.POWERBI_CLIENT_SECRET,
                scope: 'https://analysis.windows.net/powerbi/api/.default'
            }),
            {
                headers: {
                    'Content-Type': 'application/x-www-form-urlencoded'
                }
            }
        );
        
        return response.data.access_token;
    } catch (error) {
        console.error('Error getting access token:', error.response?.data || error.message);
        throw error;
    }
}

// Test the function
getAccessToken()
    .then(token => {
        console.log('Access token obtained successfully!');
        console.log('Token:', token.substring(0, 50) + '...');
    })
    .catch(error => {
        console.error('Failed to get access token:', error);
    });
```

**🎯 Hands-on Exercise:**
1. Install Node.js if you haven't already
2. Create a new folder for your project
3. Run `npm init -y`
4. Install dependencies: `npm install axios dotenv`
5. Create the `.env` file with your credentials
6. Run the script: `node auth-test.js`

---

## **Step 4: Making Your First API Calls**

### **🎯 Interactive Exercise: List Workspaces**

**Step 4.1: Create API Client**

Create a file called `powerbi-client.js`:

```javascript
const axios = require('axios');
require('dotenv').config();

class PowerBIClient {
    constructor() {
        this.baseURL = 'https://api.powerbi.com/v1.0/myorg';
        this.accessToken = null;
    }

    async authenticate() {
        try {
            const response = await axios.post(
                `https://login.microsoftonline.com/${process.env.POWERBI_TENANT_ID}/oauth2/v2.0/token`,
                new URLSearchParams({
                    grant_type: 'client_credentials',
                    client_id: process.env.POWERBI_CLIENT_ID,
                    client_secret: process.env.POWERBI_CLIENT_SECRET,
                    scope: 'https://analysis.windows.net/powerbi/api/.default'
                }),
                {
                    headers: {
                        'Content-Type': 'application/x-www-form-urlencoded'
                    }
                }
            );
            
            this.accessToken = response.data.access_token;
            console.log('✅ Authentication successful!');
        } catch (error) {
            console.error('❌ Authentication failed:', error.response?.data || error.message);
            throw error;
        }
    }

    async makeRequest(method, endpoint, data = null) {
        if (!this.accessToken) {
            await this.authenticate();
        }

        try {
            const config = {
                method,
                url: `${this.baseURL}${endpoint}`,
                headers: {
                    'Authorization': `Bearer ${this.accessToken}`,
                    'Content-Type': 'application/json'
                }
            };

            if (data) {
                config.data = data;
            }

            const response = await axios(config);
            return response.data;
        } catch (error) {
            console.error(`❌ API request failed:`, error.response?.data || error.message);
            throw error;
        }
    }

    // Workspace methods
    async getWorkspaces() {
        return await this.makeRequest('GET', '/workspaces');
    }

    async getWorkspace(id) {
        return await this.makeRequest('GET', `/workspaces/${id}`);
    }

    async createWorkspace(name) {
        return await this.makeRequest('POST', '/workspaces', { name });
    }

    // Dataset methods
    async getDatasets(workspaceId) {
        return await this.makeRequest('GET', `/workspaces/${workspaceId}/datasets`);
    }

    // Report methods
    async getReports(workspaceId) {
        return await this.makeRequest('GET', `/workspaces/${workspaceId}/reports`);
    }
}

module.exports = PowerBIClient;
```

**Step 4.2: Test Your API Client**

Create a file called `test-api.js`:

```javascript
const PowerBIClient = require('./powerbi-client');

async function testAPI() {
    const client = new PowerBIClient();
    
    try {
        console.log('🚀 Testing Power BI API...\n');
        
        // Test 1: List workspaces
        console.log('📋 Listing workspaces...');
        const workspaces = await client.getWorkspaces();
        console.log(`Found ${workspaces.value.length} workspaces:`);
        workspaces.value.forEach(ws => {
            console.log(`  - ${ws.name} (${ws.id})`);
        });
        
        // Test 2: Get details of first workspace
        if (workspaces.value.length > 0) {
            const firstWorkspace = workspaces.value[0];
            console.log(`\n📊 Getting details for workspace: ${firstWorkspace.name}`);
            const workspaceDetails = await client.getWorkspace(firstWorkspace.id);
            console.log('Workspace details:', JSON.stringify(workspaceDetails, null, 2));
            
            // Test 3: List datasets in workspace
            console.log('\n📈 Listing datasets...');
            const datasets = await client.getDatasets(firstWorkspace.id);
            console.log(`Found ${datasets.value.length} datasets`);
            
            // Test 4: List reports in workspace
            console.log('\n📊 Listing reports...');
            const reports = await client.getReports(firstWorkspace.id);
            console.log(`Found ${reports.value.length} reports`);
        }
        
        console.log('\n✅ All API tests completed successfully!');
        
    } catch (error) {
        console.error('❌ API test failed:', error);
    }
}

testAPI();
```

**🎯 Hands-on Exercise:**
1. Create both files in your project
2. Run: `node test-api.js`
3. Observe the output and understand what each API call returns

---

## **Step 5: Error Handling and Best Practices**

### **🔍 Common API Errors**

**401 Unauthorized**
- **Cause**: Invalid or expired access token
- **Solution**: Re-authenticate and get a new token

**403 Forbidden**
- **Cause**: Insufficient permissions
- **Solution**: Check API permissions in Azure AD

**404 Not Found**
- **Cause**: Resource doesn't exist
- **Solution**: Verify resource ID and workspace access

**429 Too Many Requests**
- **Cause**: Rate limiting
- **Solution**: Implement retry logic with exponential backoff

### **🎯 Interactive Exercise: Implement Error Handling**

Update your `powerbi-client.js` with better error handling:

```javascript
// Add this method to your PowerBIClient class
async makeRequestWithRetry(method, endpoint, data = null, maxRetries = 3) {
    for (let attempt = 1; attempt <= maxRetries; attempt++) {
        try {
            return await this.makeRequest(method, endpoint, data);
        } catch (error) {
            if (error.response?.status === 429 && attempt < maxRetries) {
                // Rate limited - wait and retry
                const waitTime = Math.pow(2, attempt) * 1000; // Exponential backoff
                console.log(`Rate limited. Waiting ${waitTime}ms before retry ${attempt + 1}...`);
                await new Promise(resolve => setTimeout(resolve, waitTime));
                continue;
            }
            
            if (error.response?.status === 401 && attempt < maxRetries) {
                // Token expired - re-authenticate
                console.log('Token expired. Re-authenticating...');
                this.accessToken = null;
                await this.authenticate();
                continue;
            }
            
            // Other errors or max retries reached
            throw error;
        }
    }
}
```

---

## **Step 6: Rate Limiting and Performance**

### **🔍 Power BI API Limits**
- **Requests per minute**: Varies by endpoint
- **Concurrent requests**: Limited per user/app
- **Token expiration**: 1 hour (3600 seconds)

### **🎯 Interactive Exercise: Monitor API Usage**

Add this method to track API usage:

```javascript
// Add to PowerBIClient class
constructor() {
    // ... existing code ...
    this.requestCount = 0;
    this.lastReset = Date.now();
}

async makeRequest(method, endpoint, data = null) {
    // Track requests
    this.requestCount++;
    const now = Date.now();
    
    // Reset counter every minute
    if (now - this.lastReset > 60000) {
        this.requestCount = 1;
        this.lastReset = now;
    }
    
    console.log(`📊 API Request #${this.requestCount}: ${method} ${endpoint}`);
    
    // ... rest of existing makeRequest code ...
}
```

---

## **🎯 Day 3 Summary & Quiz**

### **✅ What You Should Have Completed:**
- [ ] Understanding of REST API fundamentals
- [ ] Power BI API endpoint exploration
- [ ] Authentication setup and testing
- [ ] First API calls to list workspaces
- [ ] Error handling implementation
- [ ] Rate limiting awareness

### **🔍 Test Your Knowledge:**
1. What HTTP method would you use to create a new workspace?
2. What's the base URL for Power BI REST API?
3. How do you handle a 401 error in your API client?
4. What's the purpose of exponential backoff in retry logic?

### **🎯 Hands-on Challenge:**
Extend your API client to:
1. Create a new workspace
2. Upload a sample dataset
3. Create a simple report
4. Handle all potential errors gracefully
5. Implement proper logging

### **📚 Additional Resources:**
- [Power BI REST API Reference](https://learn.microsoft.com/en-us/rest/api/power-bi/)
- [OAuth 2.0 Client Credentials Flow](https://docs.microsoft.com/en-us/azure/active-directory/develop/v2-oauth2-client-creds-grant-flow)
- [API Best Practices](https://docs.microsoft.com/en-us/power-bi/developer/embedded/power-bi-embedded-best-practices)

**Next: Phase 2 - Embedding Power BI Content** 
