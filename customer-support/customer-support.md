# Customer Support Workshop - Technical Draft

## Introduction
This workshop builds a modern Customer Support Application using Generative AI in Oracle APEX. It covers data model creation, app generation, UI enhancements, AI assistants, and text generation.

Total Time: 60 minutes

Labs:
1. Configure AI Service
2. Create Data Model using AI
3. Create Application using Generative AI
4. Enhance UI with APEX Assistant
5. Create Support Chat Assistant
6. Generate Ticket Summary with AI
7. OPTIONAL - Enhance Charts and Database Objects with APEX Assistant

## Lab 1: Configure Generative AI Service

### Task 1: Generate API Keys (OCI)
1. Login to OCI Console.
2. Profile > Tokens and keys > Add API key > Generate API Key Pair.
3. Download Private Key, add key, copy configuration file snippet.

### Task 2: Configure Generative AI Service
1. App Builder > Workspace Utilities > All Workspace Utilities > Generative AI > Create.
2. For OCI: AI Provider: OCI Generative AI Service, Name: OCI Gen AI, Compartment ID, Region, Model ID (meta.llama-3.3-70b-instruct), Used by App Builder: ON, add credentials.
3. Test Connection > Create.

### Task 1: Generate API Keys (OpenAI)
1. Login to OpenAI account > API Keys > Create new secret key > Copy key.

### Task 2: Configure Generative AI Service
1. App Builder > Workspace Utilities > All Workspace Utilities > Generative AI > Create.
2. AI Provider: Open AI, Name: Open AI, Used by App Builder: ON, API Key, Model: gpt-3.5-turbo.
3. Test Connection > Create.

## Lab 2: Create Data Model using AI

### Task 1: Create Customer Support Data Model
1. SQL Workshop > Utilities > Create Data Model Using AI > Accept consent.
2. Choose Oracle SQL.
3. Prompt 1: Create a data model for a retail bank's customer support app with entities for Customers, their Accounts, and the Support Tickets opened for those customers.
4. Prompt 2: Add supporting entities for Service Requests, SLA tracking on each ticket, and the customer channels for support interactions.
5. Prompt 3: Prefix all objects with cs_.
6. Create SQL Script > Script Name: Customer Support Data Model.
7. APEX Assistant > Prompt: >Generate Sample Data > Copy insert queries > Paste in editor.
8. Replace with cs_data_model.sql if needed > Run.

### Task 2: Review Database Objects
1. SQL Workshop > Object Browser > Expand Tables > Select table > View details.

## Lab 3: Create Application using Generative AI

### Task 1: Refresh Data Dictionary
1. APEX Administration > Manage Service > Manage Meta Data > Data Dictionary Cache > Refresh Cache Only.

### Task 2: Create Application
1. App Builder > Create > Create APP Using Generative AI.
2. Prompt 1: Create a customer support portal for a banking customer. Create pages to manage Customers, Accounts, and Support Tickets.
3. Prompt 2: Make Support Tickets searchable using faceted search.
4. Prompt 3: Add a form page to edit Support Tickets.
5. Edit Dashboard page > Rename to Support Dashboard > Set as Home Page > Upload icon > Create Application.

### Task 3: Run Application
1. Run Application > Login > Explore Dashboard, Customers, Accounts, Support Tickets pages.

## Lab 4: Enhance UI with APEX Assistant

### Task 1: Convert Classic Report to Content Row
1. Run app > Faceted Search page > Developer Toolbar > Page 3.
2. Support Tickets region > Type: Content Row > Source > Type: SQL Query > Open editor.
3. APEX Assistant > Prompt: Also fetch Agent Name and Account type > Insert.
4. Attributes > Settings: Overline: &TICKET_NUMBER., Title: &SUBJECT., Description: &DESCRIPTION., etc. > Avatar/Badge ON.
5. Description editor > APEX Assistant > Prompts: Generate HTML..., Use APEX substitution strings, Display it below description > Insert.
6. Actions > Create Action > Position: Title Link > Link to form page.

### Task 2: Enhance Appearance
1. Runtime Developer Toolbar > Customize > Theme Roller > Select Redwood Light > Pillar: Ocean, Layout: Floating > Appearance settings > Save as Customer Support Theme.

## Lab 5: Create Support Chat Assistant

### Task 1: Set Up Support Chat Assistant without RAG
1. Page 3 > Create Button: CASE_ASSISTANT, Label: Support Assistant, Icon: fa-headset.
2. Create Dynamic Action: Show AI Assistant, Service: YOUR_GEN_AI_SERVICE, Welcome Message, Title: Support Assistant.
3. Save and Run > Test button (returns web search results).

### Task 2: Create AI Configuration and RAG Source
1. Shared Components > Generative AI > AI Configurations > Create: Name: Ticket AI Configuration, Service, System Prompt, Welcome Message.
2. RAG Sources > Create RAG Source: Name: Support Assistant Source, SQL Query via APEX Assistant > Prompt: Fetch ticket number... > Insert > Create.

### Task 3: Enable Support Chat Assistant with RAG
1. Edit Page 3 > Dynamic Action > Configuration: Ticket AI Configuration, Quick Actions messages.
2. Save and Run > Test with RAG (database results).

## Lab 6: Generate Ticket Summary with AI

### Task 1: Add Generate Summary Button
1. Ticket form page > Developer Toolbar > Page 11 > Create Button: GENERATE_SUMMARY, Label: Generate Summary, Icon: fa-file-text > Position under description item.

### Task 2: Define RAG Source
1. Shared Components > AI Configurations > Ticket AI Configuration > RAG Sources > Create: Name: Generate Ticket Summary, SQL Query with provided code.

### Task 3: Add Generate Text with AI Dynamic Action
1. Edit Page 11 > GENERATE_SUMMARY > Create Dynamic Action: Generate Text with AI, Configuration: Ticket AI Configuration, RAG Source: Generate Ticket Summary, Input: P11_ID, Use Response: P11_DESCRIPTION.
2. P11_ID > Storage: Per Session (Persistent).
3. Run app > Open ticket > Generate Summary > Apply Changes.

## Lab 7: OPTIONAL - Enhance Charts and Database Objects with APEX Assistant

### Task 1: Enhance Charts
1. Support Dashboard page > Developer Toolbar > Page 1 > Support Tickets region > Source: SQL Query > APEX Assistant > Prompt: Provide breakdown of tickets by status > Insert.
2. Prompt: Sort highest count first > Insert > Validate > Column Mapping.
3. Support Tickets By Month chart > SQL Query > APEX Assistant > Prompt: Show number of tickets created each month... > Insert > Column Mapping.

### Task 2: Create PL/SQL Package
1. SQL Workshop > SQL Commands > Paste CREATE TABLE for CS_TICKET_UPDATES.
2. APEX Assistant > Prompt: Generate PL/SQL package... > Insert Package Spec > Run > Insert Package Body > Run > Object Browser > View package.
