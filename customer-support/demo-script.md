# Customer Support Application Demo Script

## Demo Overview
**Title:** Building a Customer Support Portal with Oracle APEX and AI



## Section 1: Configure AI Service

We'll configure the AI service for APEX by setting up OCI Generative AI or OpenAI, testing the connection, and enabling it for App Builder.

*[Show both configuration paths]*

**Demo Steps:**
1. Navigate to Workspace Utilities → Generative AI
2. Configure OCI Gen AI service (if available) OR OpenAI
3. Test connection
4. Enable "Used by App Builder"

---

## Section 2: Generate Data Model with AI

We will start by generating the data model for the Customer Support Application, and we use AI to create the database schema with tables for customers, accounts, tickets, and related entities.

*[Navigate to SQL Workshop → Utilities → Create Data Model Using AI]*

**Demo Steps:**
1. Accept AI consent
2. Enter natural language prompts:
   1. "Create a data model for customer support app for a retail bank with entities for Customers, their Accounts, and the Support Tickets opened for those customers."
   2. "Add supporting entities for Service Requests, SLA tracking on each ticket, and the customer channels for support interactions."
   3. "Prefix all objects with cs_"
3. Generate SQL script
4. Use APEX Assistant to generate sample data
5. Run the complete data model

---

## Section 3: Create Application with Generative AI

Once the data model is ready, we can now move on to generating the application blueprint using AI. We simply describe our Customer Support App requirements in natural language and APEX AI Assistant creates the complete application structure.

*[Navigate to App Builder → Create → Create App Using Generative AI]*

**Demo Steps:**
1. Refresh data dictionary
2. Use prompts to define the application:
   - "Create a customer support portal for a banking customer. Create pages to manage Customers, Accounts, and Tickets."
   - "Make Tickets searchable using faceted search."
   - "Add a form page to edit Tickets."
   - "Add a Dashboard page to track customer support tickets."
3. Customize appearance (upload App icon)
4. Run the application

---

## Section 4: Enhanced Charts with AI

We'll enhance charts in the dashboard using AI by generating chart queries with APEX Assistant.

*[Navigate to dashboard and SQL Commands]*

**Demo Steps:**
1. Use APEX Assistant to generate chart queries:
   - "Provide a breakdown of tickets by status" (creates a pie chart showing ticket distribution by status)
   - "Show the number of tickets created each month over the last 12 months" (creates a bar chart showing monthly ticket trends)

---

## Section 5: Enhance UI with APEX Assistant

We'll enhance the UI on the Tickets page using APEX AI Assistant by converting the classic report to Content Row layout, enhancing the SQL query with agent names and account types, generating HTML for rich descriptions using AI, adding title links and badges, and customizing the theme with Theme Roller.

*[Navigate to the Tickets page in Page Designer]*

**Demo Steps:**
1. Convert classic report to Content Row layout
2. Use APEX Assistant to enhance SQL query (add agent names, account types)
3. Generate HTML for rich descriptions using AI
4. Add title links and badges
5. Customize theme with Theme Roller (Redwood Light, Ocean pillar)

---

## Section 6: Build AI-powered Support Chat Assistant

Now we are going to extend the app functionality by adding a fully functional AI-powered support chat assistant that leverages retrieval-augmented generation (RAG) to provide intelligent answers about ticket data, enabling users to query support information conversationally.

*[Create AI Configuration and RAG Source]*

**Demo Steps:**
1. Create AI Configuration with RAG (Retrieval-Augmented Generation)
2. Define RAG Source with SQL query for ticket data
   - Use APEX AI Assistant to generate SQL query for the RAG source: "Fetch customer support ticket details such as ticket number, subject, description, status, priority, channel, agent name, customer name, and account type."
3. Add "Ticket Assistant" button to the page
4. Configure Show AI Assistant dynamic action to use the AI Configuration
5. Test the chatbot with questions like:
   - "List high priority tickets"
   - "Show tickets assigned to Agent Rita"

---

## Section 7: Generate Ticket Summaries with AI

We'll generate ticket summaries using AI by adding a Generate Summary button, creating a RAG source for ticket details, configuring the Generate Text with AI dynamic action, and testing with different tickets.

*[Navigate to ticket landing page]*

**Demo Steps:**
1. Add "Generate Summary" button
2. Create RAG source for ticket details
3. Configure "Generate Text with AI" dynamic action
4. Test by generating summaries for different tickets

---
