# Generate Case Summary with AI

## Introduction

In this lab, you will enhance your Oracle APEX application by integrating AI-powered text generation. Specifically, you will create a feature that automatically generates case summaries based on case details stored in the database. By leveraging a Retrieval-Augmented Generation (RAG) source and dynamic actions, you will see how APEX makes it easy to combine low-code development with AI services to deliver smarter, user-friendly applications.

Estimated Time: 5 minutes

### Objectives

- Create a Generate Summary button within the case form page.

- Configure a RAG Source to fetch case details for AI input.

- Implement a Generate Text with AI dynamic action to produce summaries automatically.

## Task 1: Add Generate Summary Button

1. In the app, click any case number to open the Case Details dialog. Then, from the developer toolbar, navigate to **Page 11** (or your case form page).

    >Note: Page number may vary depending on your application.

    !["Click App Builder"](images/navigate-to-page1.png "")

2. In the **Rendering** tab, right-click **Region Body** and click **Create Button**.

    !["Click App Builder"](images/create-button-ai.png "")

3. In the Property Editor, enter/select the following:

    - Under Identification:

        - Button Name: **GENERATE_SUMMARY**

        - Label: **Generate Summary**

    - Under Appearance:

        - Button Template: **Text with Icon**

        - Hot: Toggle **On**

        - Icon: **fa-file-text**

4. Drag and drop **GENERATE_SUMMARY** button under **P11_DESCRIPTION** (or your summary/notes page item).

    >Note: Page Item number may vary depending on your application.

    !["Click App Builder"](images/generate-desc.png "")

5. Click **Save**.

## Task 2: Define RAG Source

1. Navigate to **Shared Components**.

    !["Click App Builder"](images/nav-sc.png "")

2. Under Generative AI, click **AI Configurations**.

    !["Click App Builder"](images/ai-conf3.png "")

3. Select **Case AI Configuration** (created in the previous lab). If you haven’t created it yet, follow Lab 5 to set it up first.

    !["Click App Builder"](images/event-ai-cong2.png "")

4. Under RAG Sources, click **Create RAG Source**.

    !["Click App Builder"](images/create-rag2.png "")

5. In the RAG Source page, enter/select the following:

    - Identification > Name: **Generate Case Summary**

    - Description: **Retrieve case details for summary generation**

    - Source > SQL Query: Copy and paste the following SQL into the code editor.

        ```
        <copy>
        select c.case_id,
               c.case_number,
               c.subject,
               c.description,
               c.status,
               c.priority,
               c.channel,
               cu.customer_name,
               cu.customer_type,
               ca.account_type,
               ag.agent_name,
               c.created_at,
               c.updated_at
          from cs_cases c
          join cs_customers cu on cu.customer_id = c.customer_id
          left join cs_accounts ca on ca.account_id = c.account_id
          left join cs_agents ag on ag.agent_id = c.assigned_agent_id
         where c.case_id = :P11_ID
        </copy>
        ```

    >Note: Adjust the page item reference (e.g., :P11_ID) if your form uses a different item for the case identifier.

6. Click **Create**.

    !["Click App Builder"](images/gen-desc.png "")

## Task 3: Add Generate Text with AI Dynamic Action

1. On the top right corner, click **Edit Page 11**.

    >Note: Page number may vary depending on your application.

    !["Click App Builder"](images/edit-page11.png "")

2. In the **Rendering** tab, right-click **GENERATE_SUMMARY** and select **Create Dynamic Action**.

    !["Click App Builder"](images/create-desc-dy.png "")

3. In the Property Editor, enter the following:

    - Identification > Name : **Generate Case Summary**

    !["Click App Builder"](images/geb-desc.png "")

4. Under **True** Action, click **Show**. In the Property Editor, enter/select the following:

    - Identification > Action: **Generate Text with AI**

    - Generative AI > Configuration: **Case AI Configuration**

    - RAG Source: **Generate Case Summary**

    - Under Input Value:

        - Type: **Item**

        - Item: **P11_ID**

    - Under Use Response:

        - Type: **Item**

        - Item: **P11_DESCRIPTION** (or the field you want to populate with the generated text)

    >Note: Page Item numbers may vary depending on your application.

5. Click **Save**.

    !["Click App Builder"](images/gen-text.png "")

6. Navigate to **Rendering** tab, select **P11_ID** and update the following, then click **Save**.

    - Session State > Storage: **Per Session (Persistent)**

    *Note: This ensures the case ID value persists across requests for the AI call.*

    !["Click App Builder"](images/per-session.png "")

7. Run the application and open the case form page. Click **Generate Summary** to create a case summary in the Description item. Click **Apply Changes** to save it to the table.

    !["Click App Builder"](images/view-desc-btn.png "")

## Summary

In this lab, you built an AI-powered feature to generate case summaries in Oracle APEX. You added a button, configured a RAG Source, and created a dynamic action to display and save the generated summary.

## Acknowledgments

- **Author** - Ankita Beri, Senior Product Manager
- **Last Updated By/Date** - Ankita Beri, Senior Product Manager, November 2025
