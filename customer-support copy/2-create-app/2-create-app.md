# Build the Customer Support Application using Generative AI

## Introduction

In this lab, you will create an interactive Customer Support portal using the **Create Application using Generative AI** feature in Oracle APEX. The application leverages the comprehensive customer support data model you generated in the previous lab so you can rapidly prototype end-to-end support operations.

Estimated Time: 5 minutes

### Objectives

- Create a customer-support-focused application with the AI-driven Create Application wizard based on the previously generated Customer Support data model.

### Prerequisites

- All previous labs are completed.

## Task 1: Refresh Data Dictionary

When we generate a data model, the database updates instantly but APEX’s schema metadata doesn’t. Refreshing the Data Dictionary ensures APEX reads the latest tables and makes them available in wizards and builders. In this task, we will refresh the Data Dictionary to synchronize APEX with the updated customer-support schema.

1. To refresh database objects, navigate to APEX Administration beside your user profile. Select **Manage Service > Manage Service**.

    !["Click App Builder"](images/manage-service.png "")

2. On the right hand side, under **Manage Meta Data**, select **Data Dictionary Cache**.

    !["Click App Builder"](images/data-dictionary-cache.png "")

3. To refresh the cache manually click **Refresh Cache Only**.

    !["Click App Builder"](images/refresh-cache-only.png "")

4. Now you will view refreshed cache for tables.

    !["Click App Builder"](images/review-tables.png "")

## Task 2: Create the Application using Generative AI

In this task, you will create an application using Generative AI and enter natural language.

1. From your APEX workspace top navigation bar, click **App Builder**.

    !["Click App Builder"](images/app-builder-2.png "")

2. Click **Create**.

    !["Click create App"](images/create-new-app.png "")

3. In the Create an Application page, select **Create APP Using Generative AI**.

    !["Click create app using Gen AI"](images/create-app-using-gen-ai.png "")

    *Note*: If the AI Assistant does not detect the tables created using AI, refresh the Data Dictionary Cache to ensure the latest tables are available. [Refer to the documentation for steps.](https://docs.oracle.com/en/database/oracle/apex/24.2/aeadm/accessing-data-dictionary-cache-from-administration-services.html#GUID-E398AC8D-2054-4B10-A49C-E6AD49DCF78F)

4. To create the Customer Support application, guide the APEX Assistant wizard with the prompts below. Enter each prompt and hit **Enter**.

    Prompt 1:

    ```
    <copy>
    Create a customer support portal for a banking customer. Create pages to manage Customers, Accounts, and Cases.
    </copy>
    ```

    !["first prompt"](images/app-prompt.png "")

    *Important Note:* The prompt may not always generate all pages or include every table. This behavior can vary depending on the AI service provider. If some pages are missing, you can ask the AI service to update your blueprint by specifying the additional pages you need.

5. Make sure cases can be filtered efficiently. Enter the prompt mentioned below and hit **Enter**.

    Prompt 2:
    ```
    <copy>
    Make Cases searchable using faceted search.
    </copy>
    ```

    !["second prompt"](images/edit-form.png "")

6. Finally, add the form that lets support agents edit cases.

    Prompt 3:
    ```
    <copy>
    Add a form page to edit Cases.
    </copy>
    ```

    !["third prompt"](images/enable-feedback.png "")

7. Once you are satisfied with the AI generated application blueprint, click **Create Application**.

    !["sixth prompt"](images/create-new-appp.png "")

    *Important Note:* The pages might differ based on the prompt. Make sure the blueprint includes a **Support Dashboard** page that you can later set as the Home page, a **Faceted Search** page on **CS\/\_CASES**, and a **Form** page on **CS\/\_CASES**. If something is missing, add another natural-language prompt before generating the app.*

8. On the Create an Application page, locate the generated **Dashboard** page (for example, **Insights Hub**) and click **Edit**.

    ![select appearance](images/edit-dash.png " ")

9. In the **Add Dashboard Page** dialog, rename the page to **Support Dashboard**.

10. Under **Advanced**, enable **Set as Home Page** so the Support Dashboard becomes the landing page, then click **Save Changes**.

    ![select appearance](images/update-dash.png " ")

11. Update the application name to **Customer Support Portal** and click on the app icon to upload a custom support icon.

    ![select appearance](images/set-icon.png " ")

12. Download the icon from **[here](https://c4u04.objectstorage.us-ashburn-1.oci.customer-oci.com/p/EcTjWk2IuZPZeNnD_fYMcgUhdNDIDA6rt9gaFj_WZMiL7VvxPBNMY60837hu5hga/n/c4u04/b/livelabsfiles/o/labfiles%2FAICAMP.png)** (or substitute your organization’s own asset). Click **Upload your own icon**, choose the image, and click **Save Icon**.

    ![select appearance](images/save-icon.png " ")

13. Make sure that the **Progressive Web App** and **Feedback** features remain enabled so the support app is installable and gathers agent/customer input. Then, click **Create Application**.

    ![click create application](images/create-event-app.png " ")

    ![click create application](images/create-event-app1.png " ")

## Task 3: Run the Application

1. It is now time to see how the AI generated customer-support app looks! Click **Run Application**.

    ![run the application](images/run-appp.png " ")

2. The login page will be displayed. Enter your **Username** and **Password**, then click **Sign In** to launch the Customer Support Portal.

    ![input login credentials](images/login-detail.png " ")

3. Explore key experiences:

    - Review the **Support Dashboard** home page and verify top-level KPIs render correctly.
    - Navigate to the **Customers**, **Accounts**, and any other data-model pages (such as **Channels**, **Priorities**, or **Service Levels**) to ensure record sets and quick actions work as expected.
    - Open the **Cases** faceted search page and try different filters to confirm the AI-generated facets align with your schema.
    - Use the **Case maintenance form** to view or edit a record and confirm the layout meets your agents’ needs.

    Records are pre-populated across these pages so you can validate layouts, filters, and forms without additional data entry. Update or extend the data later to reflect your real customer-support scenarios.

    ![display project dashboard page](images/event-dashboard1.png " ")

## Summary

You now know how to utilize Generative AI to create the first cut of a Customer Support application that manages your full support data model end to end.

## Acknowledgments

- **Author** - Ankita Beri, Senior Product Manager
- **Last Updated By/Date** - Ankita Beri, Senior Product Manager, November 2025
