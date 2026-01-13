# Customer Support Application Demo Script

## Demo Overview
**Title:** Building a Customer Support Portal with Oracle APEX and AI

**Duration:** 45-60 minutes

**Audience:** Developers, IT Leaders, Business Stakeholders

**Key Takeaways:**
- Learn APEX AI configuration and usage
- See AI-assisted data model generation
- Understand AI-powered application creation
- Explore APEX Assistant for UI enhancement
- Review AI chat assistant and text generation features

---

## Introduction (2 minutes)

**Narrator:** "Today we'll build a customer support application using Oracle APEX. The application will manage customer tickets, agents, accounts, and include AI-powered features.

We'll start by configuring AI services to enable APEX AI capabilities."

---

## Section 1: Configure AI Service (3 minutes)

**Narrator:** "Next, we'll configure the AI service for APEX. We'll navigate to Workspace Utilities → Generative AI, configure either OCI Generative AI or OpenAI, test the connection, and enable it for App Builder."

*[Show both configuration paths]*

**Live Demo Steps:**
1. Navigate to Workspace Utilities → Generative AI
2. Configure OCI Gen AI service (if available) OR OpenAI
3. Test connection
4. Enable "Used by App Builder"

---

## Section 2: Generate Data Model with AI (5 minutes)

**Narrator:** "Next, we'll generate the data model using AI. We'll navigate to SQL Workshop → Utilities → Create Data Model Using AI, accept the AI consent, enter natural language prompts to define the entities, generate the SQL script, create sample data with APEX Assistant, and execute the data model."

*[Navigate to SQL Workshop → Utilities → Create Data Model Using AI]*

**Live Demo Steps:**
1. Accept AI consent
2. Enter natural language prompts:
   - "Create a data model for customer support app for a retail bank with entities for Customers, their Accounts, and the Support Tickets opened for those customers."
   - "Add supporting entities for Service Requests, SLA tracking on each ticket, and the customer channels for support interactions."
   - "Prefix all objects with cs_"
3. Generate SQL script
4. Use APEX Assistant to generate sample data
5. Run the complete data model

---

## Section 3: Create Application with Generative AI (8 minutes)

**Narrator:** "Next, we'll create the application using generative AI. We'll navigate to App Builder → Create → Create App Using Generative AI, refresh the data dictionary, enter prompts to define the required pages and features, customize the appearance, and run the application."

*[Navigate to App Builder → Create → Create App Using Generative AI]*

**Live Demo Steps:**
1. Refresh data dictionary
2. Use prompts to define the application:
   - "Create a customer support portal for a banking customer. Create pages to manage Customers, Accounts, and Tickets."
   - "Make Tickets searchable using faceted search."
   - "Add a form page to edit Tickets."
   - "Add a Dashboard page to track customer support tickets."
3. Customize appearance (upload support icon, set theme)
4. Run the application

---

## Section 4: Enhanced Charts with AI (3 minutes)

**Narrator:** "Next, we'll enhance charts using AI. We'll navigate to the dashboard and SQL Commands, use APEX Assistant to generate chart queries, and create a PL/SQL package for ticket updates management."

*[Navigate to dashboard and SQL Commands]*

**Live Demo Steps:**
1. Use APEX Assistant to generate chart queries
2. Create PL/SQL package for ticket updates management

---

## Section 5: Enhance UI with APEX Assistant (7 minutes)

**Narrator:** "Next, we'll enhance the UI using APEX Assistant. We'll navigate to the Tickets page in Page Designer, convert the classic report to Content Row layout, enhance the SQL query with agent names and account types, generate HTML for rich descriptions, add title links and badges, and customize the theme using Theme Roller."

*[Navigate to the Tickets page in Page Designer]*

**Live Demo Steps:**
1. Convert classic report to Content Row layout
2. Use APEX Assistant to enhance SQL query (add agent names, account types)
3. Generate HTML for rich descriptions using AI
4. Add title links and badges
5. Customize theme with Theme Roller (Redwood Light, Ocean pillar)

---

## Section 6: Build AI-powered Support Chat Assistant (8 minutes)

**Narrator:** "Next, we'll build an AI-powered support chat assistant. We'll create an AI configuration with RAG, define a RAG source with SQL query for ticket data, add a Ticket Assistant button to the page, configure the Show AI Assistant dynamic action, and test the chatbot with queries about tickets."

*[Create AI Configuration and RAG Source]*

**Live Demo Steps:**
1. Create AI Configuration with RAG (Retrieval-Augmented Generation)
2. Define RAG Source with SQL query for ticket data
3. Add "Ticket Assistant" button to the page
4. Configure Show AI Assistant dynamic action
5. Test the chatbot with questions like:
   - "List high priority tickets"
   - "Show tickets assigned to Agent Rita"

---

## Section 7: Generate Ticket Summaries with AI (6 minutes)

**Narrator:** "Finally, we'll generate ticket summaries using AI. We'll navigate to the ticket form page, add a Generate Summary button, create a RAG source for ticket details, configure the Generate Text with AI dynamic action, and test by generating summaries for different tickets."

*[Navigate to ticket form page]*

**Live Demo Steps:**
1. Add "Generate Summary" button
2. Create RAG source for ticket details
3. Configure "Generate Text with AI" dynamic action
4. Test by generating summaries for different tickets

---

## Conclusion & Q&A (3 minutes)

**Narrator:** "In 60 minutes, we've built a customer support application using Oracle APEX with AI features. We covered data model generation, application creation, UI enhancement, chat assistant implementation, and ticket summary generation.

**What we accomplished:**
- ✅ Data model generated with AI
- ✅ Application created using generative AI
- ✅ UI enhanced with APEX Assistant
- ✅ AI-powered chat assistant with RAG
- ✅ Automated ticket summary generation
- ✅ Charts and database objects created with AI"

---

## Demo Tips & Notes

**Technical Setup:**
- Ensure AI service is pre-configured
- Have sample data loaded
- Test all features before demo
- Use high-contrast theme for visibility

**Pacing:**
- Spend more time on AI features (Sections 2, 3, 5, 6)
- Show contrast between manual vs AI approaches
- Demonstrate real user interactions

**Backup Plans:**
- Have screenshots ready if live demo fails
- Pre-recorded segments for complex setups
- Alternative scenarios if AI responses vary

**Demo Notes:**
- Focus on technical implementation steps
- Highlight APEX AI capabilities and configuration
- Demonstrate practical usage of each feature
