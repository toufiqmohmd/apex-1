# Create a Data Model using Generative AI

## Introduction

In this lab, you learn how to create a Customer Support data model using Generative AI. Ensure you have a secure key for accessing OCI Generative AI, OpenAI or Cohere services. You will then use Generative AI to create a schema that includes customers, accounts, agents, support tickets, ticket timelines, interactions, and SLAs for managing support operations.

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
    Create a data model for customer support app for a retail bank with entities for Customers, their Accounts, and the Support Tickets opened for those customers.
    </copy>
    ```

    !["provide prompt"](images/provide-prompt1.png "")

6. Enter another prompt to add supporting entities.

    **Prompt 2:**
    ```
    <copy>
    Add supporting entities for Service Requests, SLA tracking on each support ticket, and the customer channels for support interactions.
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
    -- DROP TABLE cs_ticket_updates CASCADE CONSTRAINTS;
    -- DROP TABLE cs_tickets CASCADE CONSTRAINTS;
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
    -- Support users handling tickets
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
    -- TABLE: cs_tickets
    -- The central support ticket entity
    -- ============================================================================
    CREATE TABLE cs_tickets (
        ticket_id           NUMBER GENERATED BY DEFAULT ON NULL AS IDENTITY
                        CONSTRAINT cs_tickets_id_pk PRIMARY KEY,
        ticket_number       VARCHAR2(50 CHAR) NOT NULL UNIQUE,
        customer_id         NUMBER NOT NULL
                        CONSTRAINT cs_tickets_customer_id_fk
                        REFERENCES cs_customers,
        account_id          NUMBER
                        CONSTRAINT cs_tickets_account_id_fk
                        REFERENCES cs_accounts,
        subject             VARCHAR2(500 CHAR) NOT NULL,
        description         VARCHAR2(4000 CHAR),
        priority            VARCHAR2(20 CHAR) DEFAULT 'Medium'
                        CONSTRAINT cs_tickets_priority_chk 
                        CHECK (priority IN ('Low', 'Medium', 'High', 'Critical')),
        status              VARCHAR2(50 CHAR) DEFAULT 'New'
                        CONSTRAINT cs_tickets_status_chk 
                        CHECK (status IN ('New', 'In Progress', 'Waiting', 'Resolved', 'Closed', 'Cancelled')),
        channel             VARCHAR2(50 CHAR) DEFAULT 'Email'
                        CONSTRAINT cs_tickets_channel_chk 
                        CHECK (channel IN ('Phone', 'Email', 'Chat', 'Branch', 'Mobile App', 'Web')),
        assigned_agent_id   NUMBER
                        CONSTRAINT cs_tickets_agent_id_fk
                        REFERENCES cs_agents,
        created_at          DATE NOT NULL,
        created_by          VARCHAR2(255 CHAR) NOT NULL,
        updated_at          DATE NOT NULL,
        updated_by          VARCHAR2(255 CHAR) NOT NULL,
        closed_at           DATE
    );

    -- Indexes for better query performance
    CREATE INDEX cs_tickets_i1 ON cs_tickets (customer_id);
    CREATE INDEX cs_tickets_i2 ON cs_tickets (account_id);
    CREATE INDEX cs_tickets_i3 ON cs_tickets (assigned_agent_id);
    CREATE INDEX cs_tickets_i4 ON cs_tickets (status);
    CREATE INDEX cs_tickets_i5 ON cs_tickets (priority);
    CREATE INDEX cs_tickets_i6 ON cs_tickets (created_at);

    -- ============================================================================
    -- TABLE: cs_ticket_updates
    -- Timeline of everything that happens on a ticket
    -- ============================================================================
    CREATE TABLE cs_ticket_updates (
        update_id         NUMBER GENERATED BY DEFAULT ON NULL AS IDENTITY
                          CONSTRAINT cs_ticket_updates_id_pk PRIMARY KEY,
        ticket_id         NUMBER NOT NULL
                          CONSTRAINT cs_ticket_updates_ticket_id_fk
                          REFERENCES cs_tickets,
        update_text       VARCHAR2(4000 CHAR) NOT NULL,
        updated_by        VARCHAR2(255 CHAR) NOT NULL,
        updated_at        DATE NOT NULL
    );

    -- Index for faster ticket timeline retrieval
    CREATE INDEX cs_ticket_updates_i1 ON cs_ticket_updates (ticket_id);
    CREATE INDEX cs_ticket_updates_i2 ON cs_ticket_updates (updated_at);

    -- ============================================================================
    -- TABLE: cs_interactions
    -- Stores customer conversations across channels
    -- ============================================================================
    CREATE TABLE cs_interactions (
        interaction_id    NUMBER GENERATED BY DEFAULT ON NULL AS IDENTITY
                          CONSTRAINT cs_interactions_id_pk PRIMARY KEY,
        ticket_id         NUMBER NOT NULL
                          CONSTRAINT cs_interactions_ticket_id_fk
                          REFERENCES cs_tickets,
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
    CREATE INDEX cs_interactions_i1 ON cs_interactions (ticket_id);
    CREATE INDEX cs_interactions_i2 ON cs_interactions (created_at);

    -- ============================================================================
    -- TABLE: cs_sla
    -- Simplified SLA tracking
    -- ============================================================================
    CREATE TABLE cs_sla (
        sla_id              NUMBER GENERATED BY DEFAULT ON NULL AS IDENTITY
                            CONSTRAINT cs_sla_id_pk PRIMARY KEY,
        ticket_id           NUMBER NOT NULL UNIQUE
                            CONSTRAINT cs_sla_ticket_id_fk
                            REFERENCES cs_tickets,
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

    -- Note: ticket_id already has a unique constraint, so no additional index needed
    CREATE INDEX cs_sla_i1 ON cs_sla (sla_status);

    -- ============================================================================
    -- TRIGGERS: Audit columns (created_at, created_by, updated_at, updated_by)
    -- ============================================================================

    CREATE OR REPLACE TRIGGER cs_customers_biu
        BEFORE INSERT OR UPDATE ON cs_customers
        FOR EACH ROW
    BEGIN
        IF INSERTING THEN
            :new.created_at := SYSDATE;
            :new.created_by := COALESCE(SYS_CONTEXT('APEX$SESSION','APP_USER'), USER);
        END IF;
        :new.updated_at := SYSDATE;
        :new.updated_by := COALESCE(SYS_CONTEXT('APEX$SESSION','APP_USER'), USER);
    END cs_customers_biu;
    /

    CREATE OR REPLACE TRIGGER cs_accounts_biu
        BEFORE INSERT OR UPDATE ON cs_accounts
        FOR EACH ROW
    BEGIN
        IF INSERTING THEN
            :new.created_at := SYSDATE;
            :new.created_by := COALESCE(SYS_CONTEXT('APEX$SESSION','APP_USER'), USER);
        END IF;
        :new.updated_at := SYSDATE;
        :new.updated_by := COALESCE(SYS_CONTEXT('APEX$SESSION','APP_USER'), USER);
    END cs_accounts_biu;
    /

    CREATE OR REPLACE TRIGGER cs_agents_biu
        BEFORE INSERT OR UPDATE ON cs_agents
        FOR EACH ROW
    BEGIN
        IF INSERTING THEN
            :new.created_at := SYSDATE;
            :new.created_by := COALESCE(SYS_CONTEXT('APEX$SESSION','APP_USER'), USER);
        END IF;
        :new.updated_at := SYSDATE;
        :new.updated_by := COALESCE(SYS_CONTEXT('APEX$SESSION','APP_USER'), USER);
    END cs_agents_biu;
    /

    CREATE OR REPLACE TRIGGER cs_tickets_biu
        BEFORE INSERT OR UPDATE ON cs_tickets
        FOR EACH ROW
    BEGIN
        IF INSERTING THEN
            :new.created_at := SYSDATE;
            :new.created_by := COALESCE(SYS_CONTEXT('APEX$SESSION','APP_USER'), USER);
            -- Auto-generate ticket number if not provided
            IF :new.ticket_number IS NULL THEN
                :new.ticket_number := 'TICKET-' || TO_CHAR(SYSDATE, 'YYYYMMDD') || '-' || :new.ticket_id;
            END IF;
        END IF;
        :new.updated_at := SYSDATE;
        :new.updated_by := COALESCE(SYS_CONTEXT('APEX$SESSION','APP_USER'), USER);
        
        -- Set closed_at when status changes to Closed or Resolved
        IF :new.status IN ('Closed', 'Resolved') AND :old.status NOT IN ('Closed', 'Resolved') THEN
            :new.closed_at := SYSDATE;
        END IF;
    END cs_tickets_biu;
    /

    CREATE OR REPLACE TRIGGER cs_sla_biu
        BEFORE INSERT OR UPDATE ON cs_sla
        FOR EACH ROW
    BEGIN
        IF INSERTING THEN
            :new.created_at := SYSDATE;
            :new.created_by := COALESCE(SYS_CONTEXT('APEX$SESSION','APP_USER'), USER);
        END IF;
        :new.updated_at := SYSDATE;
        :new.updated_by := COALESCE(SYS_CONTEXT('APEX$SESSION','APP_USER'), USER);
    END cs_sla_biu;
    /

    -- ============================================================================
    -- TRIGGER: Auto-create timeline entry when ticket is created
    -- ============================================================================
    CREATE OR REPLACE TRIGGER cs_tickets_timeline_ai
        AFTER INSERT ON cs_tickets
        FOR EACH ROW
    BEGIN
        INSERT INTO cs_ticket_updates (ticket_id, update_text, updated_by, updated_at)
        VALUES (
            :new.ticket_id,
            'Ticket created with priority: ' || :new.priority || ', status: ' || :new.status,
            :new.created_by,
            SYSDATE
        );
    END cs_tickets_timeline_ai;
    /

    -- ============================================================================
    -- TRIGGER: Auto-create timeline entry when ticket status or assignment changes
    -- ============================================================================
    CREATE OR REPLACE TRIGGER cs_tickets_timeline_au
        AFTER UPDATE ON cs_tickets
        FOR EACH ROW
    DECLARE
        v_update_text VARCHAR2(4000);
    BEGIN
        -- Track status changes
        IF :new.status != :old.status THEN
            v_update_text := 'Status changed from ' || :old.status || ' to ' || :new.status;
            INSERT INTO cs_ticket_updates (ticket_id, update_text, updated_by, updated_at)
            VALUES (:new.ticket_id, v_update_text, :new.updated_by, SYSDATE);
        END IF;
        
        -- Track assignment changes
        IF NVL(:new.assigned_agent_id, -1) != NVL(:old.assigned_agent_id, -1) THEN
            IF :old.assigned_agent_id IS NULL THEN
                v_update_text := 'Ticket assigned to agent ID: ' || :new.assigned_agent_id;
            ELSIF :new.assigned_agent_id IS NULL THEN
                v_update_text := 'Ticket unassigned from agent ID: ' || :old.assigned_agent_id;
            ELSE
                v_update_text := 'Ticket reassigned from agent ID: ' || :old.assigned_agent_id || 
                               ' to agent ID: ' || :new.assigned_agent_id;
            END IF;
            INSERT INTO cs_ticket_updates (ticket_id, update_text, updated_by, updated_at)
            VALUES (:new.ticket_id, v_update_text, :new.updated_by, SYSDATE);
        END IF;
        
        -- Track priority changes
        IF :new.priority != :old.priority THEN
            v_update_text := 'Priority changed from ' || :old.priority || ' to ' || :new.priority;
            INSERT INTO cs_ticket_updates (ticket_id, update_text, updated_by, updated_at)
            VALUES (:new.ticket_id, v_update_text, :new.updated_by, SYSDATE);
        END IF;
    END cs_tickets_timeline_au;
    /

    -- ============================================================================
    -- TRIGGER: Auto-create SLA record when ticket is created
    -- ============================================================================
    CREATE OR REPLACE TRIGGER cs_tickets_sla_ai
        AFTER INSERT ON cs_tickets
        FOR EACH ROW
DECLARE
        v_response_hours NUMBER;
        v_resolution_hours NUMBER;
    BEGIN
        -- Set SLA hours based on priority
        CASE :new.priority
            WHEN 'Critical' THEN
                v_response_hours := 1;
                v_resolution_hours := 4;
            WHEN 'High' THEN
                v_response_hours := 2;
                v_resolution_hours := 8;
            WHEN 'Medium' THEN
                v_response_hours := 8;
                v_resolution_hours := 24;
            WHEN 'Low' THEN
                v_response_hours := 24;
                v_resolution_hours := 72;
            ELSE
                v_response_hours := 8;
                v_resolution_hours := 24;
        END CASE;
        
        -- Create SLA record
        INSERT INTO cs_sla (
            ticket_id,
            response_due_at,
            resolution_due_at,
            sla_status
        ) VALUES (
            :new.ticket_id,
            :new.created_at + (v_response_hours / 24),
            :new.created_at + (v_resolution_hours / 24),
            'On Track'
        );
    END cs_tickets_sla_ai;
    /

    -- ============================================================================
    -- TRIGGER: Update SLA status based on current time
    -- ============================================================================
    CREATE OR REPLACE TRIGGER cs_sla_status_check
        BEFORE UPDATE ON cs_sla
        FOR EACH ROW
    DECLARE
        v_ticket_status VARCHAR2(50);
    BEGIN
        -- Get ticket status
        SELECT status INTO v_ticket_status
        FROM cs_tickets
        WHERE ticket_id = :new.ticket_id;
        
        -- Only update SLA status if ticket is not closed or resolved
        IF v_ticket_status NOT IN ('Closed', 'Resolved') THEN
            IF SYSDATE > :new.resolution_due_at THEN
                :new.sla_status := 'Breached';
            ELSIF SYSDATE > (:new.resolution_due_at - 2/24) THEN  -- Within 2 hours of breach
                :new.sla_status := 'At Risk';
            ELSE
                :new.sla_status := 'On Track';
            END IF;
        END IF;
    END cs_sla_status_check;
    /

    CREATE OR REPLACE TRIGGER cs_ticket_updates_biu
    BEFORE INSERT OR UPDATE ON cs_ticket_updates
    FOR EACH ROW
    BEGIN
        -- Set UPDATED_AT to current timestamp
        :new.updated_at := SYSDATE;
        -- Set UPDATED_BY if not populated or on insert/update (optional, as you may already set this explicitly)
        IF :new.updated_by IS NULL THEN
            :new.updated_by := COALESCE(SYS_CONTEXT('APEX$SESSION','APP_USER'), USER);
        END IF;
    END cs_ticket_updates_biu;
    /

    CREATE OR REPLACE TRIGGER cs_interactions_biu
    BEFORE INSERT OR UPDATE ON cs_interactions
    FOR EACH ROW
    BEGIN
        IF INSERTING THEN
            :new.created_at := SYSDATE;
            :new.created_by := COALESCE(SYS_CONTEXT('APEX$SESSION','APP_USER'), USER);
        END IF;
        -- Optionally update created_at/by on updates;
        -- Comment out the next 2 lines if created_at/created_by should stay unchanged on update
        :new.created_at := NVL(:new.created_at, SYSDATE);
        :new.created_by := NVL(:new.created_by, COALESCE(SYS_CONTEXT('APEX$SESSION','APP_USER'), USER));
    END cs_interactions_biu;
    /

    -- ============================================================================
    -- SAMPLE DATA
    -- ============================================================================

    -- Customers
    INSERT INTO cs_customers (customer_id, customer_name, customer_type, email, mobile, kyc_status)
    VALUES (1, 'Alice Smith',      'Individual', 'alice@example.com', '9123450001', 'Verified');
    INSERT INTO cs_customers (customer_id, customer_name, customer_type, email, mobile, kyc_status)
    VALUES (2, 'Beta Ltd.',        'Corporate',  'contact@betaltd.com', '9123450002', 'Pending');
    INSERT INTO cs_customers (customer_id, customer_name, customer_type, email, mobile, kyc_status)
    VALUES (3, 'Charlie Brown',    'Individual', 'charlie.b@example.com', '9123450003', 'Verified');
    INSERT INTO cs_customers (customer_id, customer_name, customer_type, email, mobile, kyc_status)
    VALUES (4, 'Delta Corp',       'Corporate',  'help@delta.com', '9123450004', 'Rejected');
    INSERT INTO cs_customers (customer_id, customer_name, customer_type, email, mobile, kyc_status)
    VALUES (5, 'Eve Miller',       'Individual', 'eve.m@gmail.com', '9123450005', 'Pending');
    INSERT INTO cs_customers (customer_id, customer_name, customer_type, email, mobile, kyc_status)
    VALUES (6, 'Foxtrot Group',    'Corporate',  'service@foxtrot.com', '9123450006', 'Verified');
    INSERT INTO cs_customers (customer_id, customer_name, customer_type, email, mobile, kyc_status)
    VALUES (7, 'Grace Hopper',     'Individual', 'grace.hopper@example.com', '9123450007', 'Verified');
    INSERT INTO cs_customers (customer_id, customer_name, customer_type, email, mobile, kyc_status)
    VALUES (8, 'Hotel Inc.',       'Corporate',  'info@hotelinc.com', '9123450008', 'Pending');
    INSERT INTO cs_customers (customer_id, customer_name, customer_type, email, mobile, kyc_status)
    VALUES (9, 'Ivy Lane',         'Individual', 'ivy.lane@example.com', '9123450009', 'Rejected');
    INSERT INTO cs_customers (customer_id, customer_name, customer_type, email, mobile, kyc_status)
    VALUES (10, 'Juliet Test',      'Individual', 'juliet@test.com', '9123450010', 'Verified');

    -- Accounts
    INSERT INTO cs_accounts (account_id, customer_id, account_number_masked, account_type, status)
    VALUES (1, 1,  'XXXX-1234', 'Savings',      'Active');
    INSERT INTO cs_accounts (account_id, customer_id, account_number_masked, account_type, status)
    VALUES (2, 2,  'XXXX-5678', 'Current',      'Active');
    INSERT INTO cs_accounts (account_id, customer_id, account_number_masked, account_type, status)
    VALUES (3, 3,  'XXXX-2345', 'Loan',         'Blocked');
    INSERT INTO cs_accounts (account_id, customer_id, account_number_masked, account_type, status)
    VALUES (4, 4,  'XXXX-6789', 'Credit Card',  'Active');
    INSERT INTO cs_accounts (account_id, customer_id, account_number_masked, account_type, status)
    VALUES (5, 5,  'XXXX-3456', 'Fixed Deposit','Inactive');
    INSERT INTO cs_accounts (account_id, customer_id, account_number_masked, account_type, status)
    VALUES (6, 6,  'XXXX-7890', 'Savings',      'Closed');
    INSERT INTO cs_accounts (account_id, customer_id, account_number_masked, account_type, status)
    VALUES (7, 7,  'XXXX-4567', 'Current',      'Active');
    INSERT INTO cs_accounts (account_id, customer_id, account_number_masked, account_type, status)
    VALUES (8, 8,  'XXXX-8901', 'Loan',         'Active');
    INSERT INTO cs_accounts (account_id, customer_id, account_number_masked, account_type, status)
    VALUES (9, 9,  'XXXX-5678', 'Credit Card',  'Inactive');
    INSERT INTO cs_accounts (account_id, customer_id, account_number_masked, account_type, status)
    VALUES (10, 10, 'XXXX-1234', 'Savings',     'Active');

    -- Agents
    INSERT INTO cs_agents (agent_id, agent_name, email, role, active_flag)
    VALUES (1, 'Rita Rao',         'rita.rao@org.com',         'L1',         'Y');
    INSERT INTO cs_agents (agent_id, agent_name, email, role, active_flag)
    VALUES (2, 'Vikas Sharma',     'vikas.sharma@org.com',     'L2',         'Y');
    INSERT INTO cs_agents (agent_id, agent_name, email, role, active_flag)
    VALUES (3, 'Sunita Patel',     'sunita.patel@org.com',     'Supervisor', 'Y');
    INSERT INTO cs_agents (agent_id, agent_name, email, role, active_flag)
    VALUES (4, 'Alex King',        'alex.king@org.com',        'L1',         'Y');
    INSERT INTO cs_agents (agent_id, agent_name, email, role, active_flag)
    VALUES (5, 'Edna Knight',      'edna.knight@org.com',      'Manager',    'Y');
    INSERT INTO cs_agents (agent_id, agent_name, email, role, active_flag)
    VALUES (6, 'Parul Mehta',      'parul.mehta@org.com',      'L2',         'N');
    INSERT INTO cs_agents (agent_id, agent_name, email, role, active_flag)
    VALUES (7, 'Mohit Jain',       'mohit.jain@org.com',       'L1',         'Y');
    INSERT INTO cs_agents (agent_id, agent_name, email, role, active_flag)
    VALUES (8, 'Tina Das',         'tina.das@org.com',         'Supervisor', 'Y');
    INSERT INTO cs_agents (agent_id, agent_name, email, role, active_flag)
    VALUES (9, 'Priya S',          'priya.s@org.com',          'L1',         'Y');
    INSERT INTO cs_agents (agent_id, agent_name, email, role, active_flag)
    VALUES (10, 'Sunil Menon',      'sunil.menon@org.com',      'L2',         'Y');

    -- Tickets (formerly Cases)
    INSERT INTO cs_tickets (ticket_id, ticket_number, customer_id, account_id, subject, description, priority, status, channel, assigned_agent_id)
    VALUES (1, 'TICKET-20240101-1', 1, 1,   'KYC document not verified',      'Customer submitted KYC docs but not verified',            'Medium',  'New',         'Email',  1);
    INSERT INTO cs_tickets (ticket_id, ticket_number, customer_id, account_id, subject, description, priority, status, channel, assigned_agent_id)
    VALUES (2, 'TICKET-20240101-2', 2, 2,   'Unable to access account',       'Corporate login fails with auth error',                   'High',    'In Progress', 'Chat',   2);
    INSERT INTO cs_tickets (ticket_id, ticket_number, customer_id, account_id, subject, description, priority, status, channel, assigned_agent_id)
    VALUES (3, 'TICKET-20240101-3', 3, 3,   'Loan account showing blocked',   'Loan EMI paid, but blocked warning in app',               'Critical','Waiting',     'Mobile App', 3);
    INSERT INTO cs_tickets (ticket_id, ticket_number, customer_id, account_id, subject, description, priority, status, channel, assigned_agent_id)
    VALUES (4, 'TICKET-20240101-4', 4, 4,   'Credit card payment pending',    'Customer reports payment not updated',                    'Low',     'New',         'Phone',  4);
    INSERT INTO cs_tickets (ticket_id, ticket_number, customer_id, account_id, subject, description, priority, status, channel, assigned_agent_id)
    VALUES (5, 'TICKET-20240101-5', 5, 5,   'Fixed deposit wrongly closed',   'FD closed without consent',                               'High',    'Resolved',    'Email',  5);
    INSERT INTO cs_tickets (ticket_id, ticket_number, customer_id, account_id, subject, description, priority, status, channel, assigned_agent_id)
    VALUES (6, 'TICKET-20240101-6', 6, 6,   'Closed account showing open',    'Savings account shows open after closure',                'Medium',  'Closed',      'Branch', 6);
    INSERT INTO cs_tickets (ticket_id, ticket_number, customer_id, account_id, subject, description, priority, status, channel, assigned_agent_id)
    VALUES (7, 'TICKET-20240101-7', 7, 7,   'Unable to withdraw cash',        'ATM not dispensing, account debited',                     'Critical','Cancelled',   'Phone',    7); 
    INSERT INTO cs_tickets (ticket_id, ticket_number, customer_id, account_id, subject, description, priority, status, channel, assigned_agent_id)
    VALUES (8, 'TICKET-20240101-8', 8, 8,   'Loan account missing in app',    'Customer cannot see loan details',                        'Medium',  'New',         'Web',    8);
    INSERT INTO cs_tickets (ticket_id, ticket_number, customer_id, account_id, subject, description, priority, status, channel, assigned_agent_id)
    VALUES (9, 'TICKET-20240101-9', 9, 9,   'Credit card limit reduced',      'Customer not informed before reduction',                  'High',    'New',         'Email',  9);
    INSERT INTO cs_tickets (ticket_id, ticket_number, customer_id, account_id, subject, description, priority, status, channel, assigned_agent_id)
    VALUES (10,'TICKET-20240101-10',10, 10, 'Unable to withdraw - savings',   'Customer unable to withdraw even after sufficient funds', 'Medium',  'In Progress', 'Chat',  10);

    -- Ticket Updates (formerly Case Updates)
    INSERT INTO cs_ticket_updates (ticket_id, update_text, updated_by)
    VALUES (1, 'KYC Pending verification.', 'agent1');
    INSERT INTO cs_ticket_updates (ticket_id, update_text, updated_by)
    VALUES (2, 'Reset password instructions sent.', 'agent2');
    INSERT INTO cs_ticket_updates (ticket_id, update_text, updated_by)
    VALUES (3, 'Escalated to backend for loan unblock', 'agent3');
    INSERT INTO cs_ticket_updates (ticket_id, update_text, updated_by)
    VALUES (4, 'Queued for card operations team', 'agent4');
    INSERT INTO cs_ticket_updates (ticket_id, update_text, updated_by)
    VALUES (5, 'Customer contacted via phone.', 'agent5');
    INSERT INTO cs_ticket_updates (ticket_id, update_text, updated_by)
    VALUES (6, 'Account closure confirmed.', 'agent6');
    INSERT INTO cs_ticket_updates (ticket_id, update_text, updated_by)
    VALUES (7, 'ATM dispute logged. Awaiting response', 'agent7');
    INSERT INTO cs_ticket_updates (ticket_id, update_text, updated_by)
    VALUES (8, 'Ticket created and assigned', 'agent8');
    INSERT INTO cs_ticket_updates (ticket_id, update_text, updated_by)
    VALUES (9, 'Complaint acknowledged', 'agent9');
    INSERT INTO cs_ticket_updates (ticket_id, update_text, updated_by)
    VALUES (10, 'Savings withdrawal issue recreated', 'agent10');

    -- cs_interactions: 10 rows, valid channel values
    INSERT INTO cs_interactions (ticket_id, channel, message_text, direction, created_by)
    VALUES (1, 'Email',   'Welcome, please verify your KYC.', 'Outbound', 'agent1');
    INSERT INTO cs_interactions (ticket_id, channel, message_text, direction, created_by)
    VALUES (1, 'Email',   'KYC documents attached as requested.', 'Inbound', 'alice@example.com');
    INSERT INTO cs_interactions (ticket_id, channel, message_text, direction, created_by)
    VALUES (2, 'Chat',    'I am unable to access my corporate account.', 'Inbound', 'contact@betaltd.com');
    INSERT INTO cs_interactions (ticket_id, channel, message_text, direction, created_by)
    VALUES (2, 'Chat',    'We are looking into your login issue.', 'Outbound', 'agent2');
    INSERT INTO cs_interactions (ticket_id, channel, message_text, direction, created_by)
    VALUES (3, 'Call',    'Loan account is showing as blocked.', 'Inbound', 'charlie.b@example.com');
    INSERT INTO cs_interactions (ticket_id, channel, message_text, direction, created_by)
    VALUES (3, 'Call',    'Your issue has been escalated for review.', 'Outbound', 'agent3');
    INSERT INTO cs_interactions (ticket_id, channel, message_text, direction, created_by)
    VALUES (4, 'Branch',  'I paid my credit card bill but it is not showing.', 'Inbound', 'help@delta.com');
    INSERT INTO cs_interactions (ticket_id, channel, message_text, direction, created_by)
    VALUES (4, 'Branch',  'Our branch team will contact you for resolution.', 'Outbound', 'agent4');
    INSERT INTO cs_interactions (ticket_id, channel, message_text, direction, created_by)
    VALUES (5, 'SMS',     'Why was my Fixed Deposit closed?', 'Inbound', 'eve.m@gmail.com');
    INSERT INTO cs_interactions (ticket_id, channel, message_text, direction, created_by)
    VALUES (5, 'SMS',     'We are investigating your Fixed Deposit issue.', 'Outbound', 'agent5');

    COMMIT;

    -- ============================================================================
    -- END OF DDL SCRIPT
    -- ============================================================================
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

- **Author** - Toufiq Mohammed, Principal Product Manager
- **Last Updated By/Date** - Toufiq Mohammed, Principal Product Manager, January 2026
