# Customer Support Application Demo Script

## Demo Overview
**Title:** Building an AI-Powered Customer Support Portal with Oracle APEX

**Duration:** 45-60 minutes

**Audience:** Developers, IT Leaders, Business Stakeholders

**Key Takeaways:**
- See how AI accelerates APEX development from concept to production
- Experience the complete customer support workflow
- Understand the blend of low-code development and AI assistance

---

## Introduction (2 minutes)

**Narrator:** "Welcome everyone! Today, we're going to witness something remarkable. In just 60 minutes, we'll build a complete, AI-powered customer support application using Oracle APEX. This isn't just any demo - it's a live demonstration of how generative AI is revolutionizing application development.

Our goal: Create a modern customer support portal for a retail bank that manages customer tickets, agents, accounts, and provides intelligent AI assistance.

Let's start with the foundation - configuring our AI services."

---

## Section 1: Configure AI Service (3 minutes)

**Narrator:** "First, we need to set up our AI capabilities. APEX supports multiple AI providers - today I'll show you both OCI Generative AI and OpenAI setup."

*[Show both configuration paths]*

**Live Demo Steps:**
1. Navigate to Workspace Utilities → Generative AI
2. Configure OCI Gen AI service (if available) OR OpenAI
3. Test connection
4. Enable "Used by App Builder"

**Key Points to Highlight:**
- "APEX makes AI setup as simple as configuring a database connection"
- "One configuration enables AI across the entire workspace"
- "Support for enterprise-grade AI services like OCI Generative AI"

---

## Section 2: Generate Data Model with AI (5 minutes)

**Narrator:** "Now comes the magic! Instead of manually designing tables for customers, accounts, agents, and support tickets, we'll use AI to generate our complete data model."

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

**Key Points to Highlight:**
- "Watch how AI understands business requirements and generates proper database design"
- "The AI creates not just tables, but relationships, constraints, and even triggers"
- "APEX Assistant helps generate realistic sample data automatically"

---

## Section 3: Create Application with Generative AI (8 minutes)

**Narrator:** "With our data model ready, let's create the entire application using AI. No coding required - just natural language descriptions."

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

**Key Points to Highlight:**
- "AI generates complete, functional pages based on your descriptions"
- "The application includes navigation, forms, reports, and dashboards"
- "Everything is immediately runnable with sample data"

---

## Section 4: Enhanced Charts with AI (3 minutes)

**Narrator:** "Now let's see how APEX Assistant helps create insightful charts and even database packages."

*[Navigate to dashboard and SQL Commands]*

**Live Demo Steps:**
1. Use APEX Assistant to generate chart queries
2. Create PL/SQL package for ticket updates management

**Key Points to Highlight:**
- "AI assistance extends to data visualization and backend logic"
- "Complete development workflow enhanced by AI"

---

## Section 5: Enhance UI with APEX Assistant (7 minutes)

**Narrator:** "Now let's see how APEX Assistant enhances our user interface. We'll transform a basic report into a modern, interactive content row layout."

*[Navigate to the Tickets page in Page Designer]*

**Live Demo Steps:**
1. Convert classic report to Content Row layout
2. Use APEX Assistant to enhance SQL query (add agent names, account types)
3. Generate HTML for rich descriptions using AI
4. Add title links and badges
5. Customize theme with Theme Roller (Redwood Light, Ocean pillar)

**Key Points to Highlight:**
- "APEX Assistant generates both SQL and HTML using natural language"
- "Theme Roller provides instant visual customization"
- "The result is a modern, mobile-friendly interface"

---

## Section 6: Build AI-powered Support Chat Assistant (8 minutes)

**Narrator:** "Let's add intelligence to our application with an AI-powered support assistant that can answer questions about tickets using our database."

*[Create AI Configuration and RAG Source]*

**Live Demo Steps:**
1. Create AI Configuration with RAG (Retrieval-Augmented Generation)
2. Define RAG Source with SQL query for ticket data
3. Add "Ticket Assistant" button to the page
4. Configure Show AI Assistant dynamic action
5. Test the chatbot with questions like:
   - "List high priority tickets"
   - "Show tickets assigned to Agent Rita"

**Key Points to Highlight:**
- "RAG ensures the AI only uses your business data, not general web knowledge"
- "The assistant provides contextual, database-driven responses"
- "This creates intelligent, context-aware user experiences"

---

## Section 7: Generate Ticket Summaries with AI (6 minutes)

**Narrator:** "Finally, let's implement AI-powered text generation that automatically creates ticket summaries based on ticket details."

*[Navigate to ticket form page]*

**Live Demo Steps:**
1. Add "Generate Summary" button
2. Create RAG source for ticket details
3. Configure "Generate Text with AI" dynamic action
4. Test by generating summaries for different tickets

**Key Points to Highlight:**
- "AI generates professional, contextual summaries automatically"
- "Reduces manual work while maintaining quality"
- "Shows how AI can enhance productivity across business processes"

---

## Conclusion & Q&A (3 minutes)

**Narrator:** "In just 60 minutes, we've built a complete, AI-powered customer support application. From data model generation to intelligent chat assistants, we've seen how Oracle APEX combines the productivity of low-code development with the power of generative AI.

**What we've accomplished:**
- ✅ Complete data model generated with AI
- ✅ Full application created using natural language
- ✅ Modern UI enhanced with AI assistance
- ✅ Intelligent chat assistant with RAG
- ✅ Automated ticket summary generation
- ✅ Professional charts and database objects

This is the future of application development - fast, intelligent, and accessible to all developers."

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

**Audience Engagement:**
- Ask what business processes they'd AI-ify
- Poll on most impressive feature
- Discuss enterprise readiness

**Key Messages:**
- "AI doesn't replace developers - it supercharges them"
- "From idea to production in hours, not months"
- "Enterprise-grade security and governance maintained"
