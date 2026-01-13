# Create a Data Model using Generative AI

## Introduction

In this lab, you learn how to create a Customer Support data model using Generative AI. Ensure you have a secure key for accessing OCI Generative AI, OpenAI or Cohere services. You will then use Generative AI to create a schema that includes customers, accounts, agents, cases, case timelines, interactions, and SLAs for managing support operations.

Estimated Time: 5 minutes

### Objectives

In this lab, you will:

- Create a Data Model using AI in Oracle APEX Workspace.

### Before You Start

- Sign up and Get access for one of the supported Generative AI Services like OCI Generative AI, OpenAI or Cohere.

- An Oracle Cloud paid account, or free trial.

- An APEX Workspace

## Task 1: Create Customer Support Data Model using AI

To create a data model with AI, ensure that you have configured Generative AI Service and enabled **Used by App Builder** (Refer to the previous Task). If a Generative AI Service is not configured, the Create Data Model Using AI option will not be visible.

In this task, you will learn how to leverage Oracle APEX's Generative AI Service to build a Customer Support Data Model without writing SQL manually. By providing simple prompts, you will generate database objects, refine them, and add sample records automatically.

1. Login to your Application. On the Workspace home page, click **SQL Workshop**.

    ![select sql workshop](./images/select-sql-workshop.png " ")

2. Click **Utilities**.

    ![select utilities](./images/select-utilities.png " ")

3. Click **Create Data Model Using AI**.

    ![select create data model](./images/click-create-data-model-ai.png " ")

    ![select create data model alternate](./images/click-create-data-model.png " ")

    >**Note:** You can also access Create Data Model Using AI directly from the Tasks list on the SQL Workshop home page.

4. When using Generative AI features within the APEX development environment *for the first time*, you will be asked to provide consent. In the **APEX Assistant** Wizard, if you see a Dialog regarding **consent**. Click on **Accept**.

    ![provide consent](./images/provide-consent.png " ")

5. You will use the **APEX Assistant** Wizard to create a Customer Support Data Model using AI. To do this, enter the prompts mentioned below. Make sure that you choose **Oracle SQL** for **SQL Format**.

    **Prompt 1:**
    ```
    <copy>
    Create a data model for a retail bank's customer support app with entities for Customers, their Accounts, and the Cases opened for those customers.
    </copy>
    ```

    !["provide prompt"](images/provide-prompt1.png "")

6. Enter another prompt to add supporting entities.

    **Prompt 2:**
    ```
    <copy>
    Add supporting entities for Service Requests, SLA tracking on each case, and the customer channels for support interactions.
    </copy>
    ```

    !["provide prompt"](images/add-event-types.png "")

7. Add another prompt to update prefix of all database objects.

    **Prompt 3:**
    ```
    <copy>
    Prefix all objects with cs_
    </copy>
    ```

    !["provide prompt"](images/prefix.png "")

8. At this point, we are satisfied with the generated SQL script. Click **Create SQL Script**.

    !["click create sql script"](images/review-quick-sql.png "")

9. For Script Name, enter **Customer Support Data Model**.

    !["provide script name"](images/event-data-model.png "")

10. Next, we'd like to add sample data into the tables. To do this, we leverage the APEX Assistant in the Code Editor. Click **APEX Assistant**.

    !["provide script name"](images/click-apex-assistant.png "")

11. Select your SQL code and click **Use Selection** from the APEX Assistant box.

    !["provide script name"](images/use-selection.png "")

12. In APEX Assistant box, enter the prompt to generate sample data for that tables.

    **Prompt 1:**
    ```
    <copy>
    >Generate Sample Data
    </copy>
    ```

    !["provide script name"](images/generate-sample-data.png "")

13. **Copy** the generated insert queries from the APEX Assistant box.

    !["provide script name"](images/copy-query.png "")

14. Paste the copied queries into the left-hand side code editor towards the end.

    !["provide script name"](images/insert-query.png "")

15. Before clicking on **Run** button, you can optionally replace the generated script with the provided `cs_data_model.sql` to ensure your objects match the reference data model used throughout this workshop. The excerpt below shows the beginning of that script:

    > Note: We are replacing the code to ensure the lab can be completed as intended. The replacement is only for consistency with the lab steps and expected results.

    ```
    <copy>
    -- ============================================================================
    -- BFSI Customer Support System - Oracle Database DDL
    -- Demo-focused simplified data model
    -- All objects prefixed with CS_ (Customer Support)
    -- ============================================================================

    -- Drop tables if they exist (for clean reinstall)
    -- DROP TABLE cs_sla CASCADE CONSTRAINTS;
    -- DROP TABLE cs_interactions CASCADE CONSTRAINTS;
    -- DROP TABLE cs_case_updates CASCADE CONSTRAINTS;
    -- DROP TABLE cs_cases CASCADE CONSTRAINTS;
    -- DROP TABLE cs_agents CASCADE CONSTRAINTS;
    -- DROP TABLE cs_accounts CASCADE CONSTRAINTS;
    -- DROP TABLE cs_customers CASCADE CONSTRAINTS;

    -- ============================================================================
    -- TABLE: cs_customers
    -- Represents the customer raising a support request
    -- ============================================================================
    CREATE TABLE cs_customers (
        customer_id       NUMBER GENERATED BY DEFAULT ON NULL AS IDENTITY
                          CONSTRAINT cs_customers_id_pk PRIMARY KEY,
        customer_name     VARCHAR2(255 CHAR) NOT NULL,
        customer_type     VARCHAR2(50 CHAR) DEFAULT 'Individual' 
                          CONSTRAINT cs_customers_type_chk 
                          CHECK (customer_type IN ('Individual', 'Corporate')),
        email             VARCHAR2(255 CHAR),
        mobile            VARCHAR2(50 CHAR),
        kyc_status        VARCHAR2(50 CHAR) DEFAULT 'Pending'
                          CONSTRAINT cs_customers_kyc_chk 
                          CHECK (kyc_status IN ('Pending', 'Verified', 'Rejected')),
        created_at        DATE NOT NULL,
        created_by        VARCHAR2(255 CHAR) NOT NULL,
        updated_at        DATE NOT NULL,
        updated_by        VARCHAR2(255 CHAR) NOT NULL
    );

    -- ============================================================================
    -- TABLE: cs_accounts
    -- Provides BFSI context like banking, loans, or cards
    -- ============================================================================
    CREATE TABLE cs_accounts (
        account_id              NUMBER GENERATED BY DEFAULT ON NULL AS IDENTITY
                                CONSTRAINT cs_accounts_id_pk PRIMARY KEY,
        customer_id             NUMBER NOT NULL
                                CONSTRAINT cs_accounts_customer_id_fk
                                REFERENCES cs_customers,
        account_number_masked   VARCHAR2(50 CHAR) NOT NULL,
        account_type            VARCHAR2(50 CHAR) NOT NULL
                                CONSTRAINT cs_accounts_type_chk 
                                CHECK (account_type IN ('Savings', 'Current', 'Loan', 'Credit Card', 'Fixed Deposit')),
        status                  VARCHAR2(50 CHAR) DEFAULT 'Active'
                                CONSTRAINT cs_accounts_status_chk 
                                CHECK (status IN ('Active', 'Inactive', 'Blocked', 'Closed')),
        created_at              DATE NOT NULL,
        created_by              VARCHAR2(255 CHAR) NOT NULL,
        updated_at              DATE NOT NULL,
        updated_by              VARCHAR2(255 CHAR) NOT NULL
    );

    -- Index on customer_id for faster lookups
    CREATE INDEX cs_accounts_i1 ON cs_accounts (customer_id);

    -- ============================================================================
    -- TABLE: cs_agents
    -- Support users handling cases
    -- ============================================================================
    CREATE TABLE cs_agents (
        agent_id          NUMBER GENERATED BY DEFAULT ON NULL AS IDENTITY
                          CONSTRAINT cs_agents_id_pk PRIMARY KEY,
        agent_name        VARCHAR2(255 CHAR) NOT NULL,
        email             VARCHAR2(255 CHAR) NOT NULL,
        role              VARCHAR2(50 CHAR) DEFAULT 'L1'
                          CONSTRAINT cs_agents_role_chk 
                          CHECK (role IN ('L1', 'L2', 'Supervisor', 'Manager')),
        active_flag       VARCHAR2(1 CHAR) DEFAULT 'Y'
                          CONSTRAINT cs_agents_active_chk 
                          CHECK (active_flag IN ('Y', 'N')),
        created_at        DATE NOT NULL,
        created_by        VARCHAR2(255 CHAR) NOT NULL,
        updated_at        DATE NOT NULL,
        updated_by        VARCHAR2(255 CHAR) NOT NULL
    );

    -- ============================================================================
    -- TABLE: cs_cases
    -- The central support ticket entity
    -- ============================================================================
    CREATE TABLE cs_cases (
        case_id             NUMBER GENERATED BY DEFAULT ON NULL AS IDENTITY
                            CONSTRAINT cs_cases_id_pk PRIMARY KEY,
        case_number         VARCHAR2(50 CHAR) NOT NULL UNIQUE,
        customer_id         NUMBER NOT NULL
                            CONSTRAINT cs_cases_customer_id_fk
                            REFERENCES cs_customers,
        account_id          NUMBER
                            CONSTRAINT cs_cases_account_id_fk
                            REFERENCES cs_accounts,
        subject             VARCHAR2(500 CHAR) NOT NULL,
        description         VARCHAR2(4000 CHAR),
        priority            VARCHAR2(20 CHAR) DEFAULT 'Medium'
                            CONSTRAINT cs_cases_priority_chk 
                            CHECK (priority IN ('Low', 'Medium', 'High', 'Critical')),
        status              VARCHAR2(50 CHAR) DEFAULT 'New'
                            CONSTRAINT cs_cases_status_chk 
                            CHECK (status IN ('New', 'In Progress', 'Waiting', 'Resolved', 'Closed', 'Cancelled')),
        channel             VARCHAR2(50 CHAR) DEFAULT 'Email'
                            CONSTRAINT cs_cases_channel_chk 
                            CHECK (channel IN ('Phone', 'Email', 'Chat', 'Branch', 'Mobile App', 'Web')),
        assigned_agent_id   NUMBER
                            CONSTRAINT cs_cases_agent_id_fk
                            REFERENCES cs_agents,
        created_at          DATE NOT NULL,
        created_by          VARCHAR2(255 CHAR) NOT NULL,
        updated_at          DATE NOT NULL,
        updated_by          VARCHAR2(255 CHAR) NOT NULL,
        closed_at           DATE
    );

    -- Indexes for better query performance
    CREATE INDEX cs_cases_i1 ON cs_cases (customer_id);
    CREATE INDEX cs_cases_i2 ON cs_cases (account_id);
    CREATE INDEX cs_cases_i3 ON cs_cases (assigned_agent_id);
    CREATE INDEX cs_cases_i4 ON cs_cases (status);
    CREATE INDEX cs_cases_i5 ON cs_cases (priority);
    CREATE INDEX cs_cases_i6 ON cs_cases (created_at);

    -- ============================================================================
    -- TABLE: cs_case_updates
    -- Timeline of everything that happens on a case
    -- ============================================================================
    CREATE TABLE cs_case_updates (
        update_id         NUMBER GENERATED BY DEFAULT ON NULL AS IDENTITY
                          CONSTRAINT cs_case_updates_id_pk PRIMARY KEY,
        case_id           NUMBER NOT NULL
                          CONSTRAINT cs_case_updates_case_id_fk
                          REFERENCES cs_cases,
        update_text       VARCHAR2(4000 CHAR) NOT NULL,
        updated_by        VARCHAR2(255 CHAR) NOT NULL,
        updated_at        DATE NOT NULL
    );

    -- Index for faster case timeline retrieval
    CREATE INDEX cs_case_updates_i1 ON cs_case_updates (case_id);
    CREATE INDEX cs_case_updates_i2 ON cs_case_updates (updated_at);

    -- ============================================================================
    -- TABLE: cs_interactions
    -- Stores customer conversations across channels
    -- ============================================================================
    CREATE TABLE cs_interactions (
        interaction_id    NUMBER GENERATED BY DEFAULT ON NULL AS IDENTITY
                          CONSTRAINT cs_interactions_id_pk PRIMARY KEY,
        case_id           NUMBER NOT NULL
                          CONSTRAINT cs_interactions_case_id_fk
                          REFERENCES cs_cases,
        channel           VARCHAR2(50 CHAR) NOT NULL
                          CONSTRAINT cs_interactions_channel_chk 
                          CHECK (channel IN ('Call', 'Chat', 'Email', 'SMS', 'Branch')),
        message_text      VARCHAR2(4000 CHAR),
        direction         VARCHAR2(20 CHAR) DEFAULT 'Inbound'
                          CONSTRAINT cs_interactions_direction_chk 
                          CHECK (direction IN ('Inbound', 'Outbound')),
        created_at        DATE NOT NULL,
        created_by        VARCHAR2(255 CHAR) NOT NULL
    );

    -- Indexes for interaction queries
    CREATE INDEX cs_interactions_i1 ON cs_interactions (case_id);
    CREATE INDEX cs_interactions_i2 ON cs_interactions (created_at);

    -- ============================================================================
    -- TABLE: cs_sla
    -- Simplified SLA tracking
    -- ============================================================================
    CREATE TABLE cs_sla (
        sla_id              NUMBER GENERATED BY DEFAULT ON NULL AS IDENTITY
                            CONSTRAINT cs_sla_id_pk PRIMARY KEY,
        case_id             NUMBER NOT NULL UNIQUE
                            CONSTRAINT cs_sla_case_id_fk
                            REFERENCES cs_cases,
        response_due_at     DATE NOT NULL,
        resolution_due_at   DATE NOT NULL,
        sla_status          VARCHAR2(20 CHAR) DEFAULT 'On Track'
                            CONSTRAINT cs_sla_status_chk 
                            CHECK (sla_status IN ('On Track', 'At Risk', 'Breached')),
        created_at          DATE NOT NULL,
        created_by          VARCHAR2(255 CHAR) NOT NULL,
        updated_at          DATE NOT NULL,
        updated_by          VARCHAR2(255 CHAR) NOT NULL
    );

    -- Note: case_id already has a unique constraint, so no additional index needed
    CREATE INDEX cs_sla_i1 ON cs_sla (sla_status);
    ```
    </copy>

16. After replacing the code, click **Run** in the Script Editor.

    !["run now"](images/confirm-yes.png "")

17. Click **Run Now** to submit the script for execution.

    !["run now"](images/run-now.png "")

18. The Manage Script Results page appears listing script results.

    !["data model created"](images/successful-statemwnts.png "")

    *Note: Do NOT click Create App yet, as you will creating an app in the upcoming lab using Generative AI.*

## Task 2: Review Database Objects

Now, let's review the database objects created using AI.

1. Navigate to **SQL Workshop** > **Object Browser**.

    ![SQL Workshop home page](./images/object-browser.png " ")

2. Expand **Tables** in the Object Pane to view different tables. Select a table to view table details such as Data, Constraints, and so forth in the Details View pane on the right.

    ![Object Browser](./images/view-tables.png " ")

## Summary

You now know how to create a Data Model using AI. You may now **proceed to the next lab**.

## Acknowledgments

- **Author** - Ankita Beri, Senior Product Manager
- **Last Updated By/Date** - Ankita Beri, Senior Product Manager, November 2025
