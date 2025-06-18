# Power BI Core Concepts - Interactive Guide

## 🏗️ **Power BI Building Blocks**

### **1. Workspaces (Your Development Environment)**
Think of workspaces as **folders** where you organize your Power BI content.

**🎯 Interactive Exercise:**
1. Go to https://app.powerbi.com
2. Click "Workspaces" in the left sidebar
3. Click "Create a workspace"
4. Name it: "My Developer Workspace"
5. Set it as "Public" (for now - we'll secure it later)

**💡 Pro Tip:** Workspaces can be:
- **My workspace** (personal)
- **Workspaces** (shared with others)
- **Premium workspaces** (advanced features)

### **2. Datasets (Your Data Foundation)**
Datasets are the **data models** that power your reports.

**🎯 Interactive Exercise:**
1. In your workspace, click "New" → "Dataset"
2. Choose "Excel" as your data source
3. Download this sample data: [Sample Sales Data](https://github.com/microsoft/PowerBI-Embedded-Samples/raw/master/QuickStart/QuickStart/Data/SalesData.xlsx)
4. Upload the file and explore the data structure

**💡 What you'll see:**
- Tables and relationships
- Data types and formatting
- Refresh settings

### **3. Reports (Your Visualizations)**
Reports are **collections of visualizations** that tell your data story.

**🎯 Interactive Exercise:**
1. In Power BI Desktop, open the sample data
2. Create 3 different visualizations:
   - A bar chart showing sales by region
   - A line chart showing sales over time
   - A table showing top 10 products
3. Save as "My First Report.pbix"
4. Publish to your workspace

### **4. Dashboards (Your Executive View)**
Dashboards are **single-page views** of your most important metrics.

**🎯 Interactive Exercise:**
1. In your workspace, click on your published report
2. Pin some visualizations to a new dashboard
3. Name it "Executive Dashboard"
4. Add a text box with a title

### **5. Apps (Your Distribution Method)**
Apps are **packaged collections** of dashboards and reports for distribution.

**🎯 Interactive Exercise:**
1. In your workspace, click "Create app"
2. Add your dashboard and report
3. Configure access permissions
4. Publish the app

---

## 🔐 **Security Concepts**

### **Row-Level Security (RLS)**
RLS controls **which data rows** users can see.

**🎯 Interactive Exercise:**
1. In Power BI Desktop, go to "Model" view
2. Right-click on a table → "Manage roles"
3. Create a role called "Sales Manager"
4. Add a filter: `[Region] = "North"`
5. Test the role

**💡 Real-world example:**
- Sales managers only see their region's data
- HR managers only see employee data for their department

---

## 📊 **Data Refresh Concepts**

### **Types of Refresh:**
1. **Scheduled refresh** - Automatic (cloud data sources)
2. **Manual refresh** - On-demand
3. **Real-time refresh** - Streaming data

**🎯 Interactive Exercise:**
1. In your workspace, go to your dataset
2. Click "Settings" → "Scheduled refresh"
3. Set up a daily refresh at 6 AM
4. Test a manual refresh

---

## 🎯 **Day 1 Summary & Quiz**

**Test Your Knowledge:**
1. What's the difference between Power BI Desktop and Power BI Service?
2. What is a workspace and why do you need one?
3. What's the relationship between datasets, reports, and dashboards?
4. How does Row-Level Security work?

**🎯 Hands-on Challenge:**
Create a complete workflow:
1. Download sample data
2. Create a report in Desktop
3. Publish to Service
4. Create a dashboard
5. Set up basic security

**Next: Day 2 - Developer Environment Setup** 
