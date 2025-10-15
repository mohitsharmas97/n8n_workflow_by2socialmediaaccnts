# n8n Social Media Automation from Google Sheets

Video Link:https://www.youtube.com/watch?v=g_Gqtg7g6VU
Workflow Link:https://mohitsharmaii.app.n8n.cloud/workflow/YvKLqFW0YgIUkcMq

This n8n workflow automates social media posting to Twitter (X) and Reddit. It uses a Google Sheet as a content scheduler, triggering the workflow every time a new row is added.

## Workflow Overview

The workflow is designed for simplicity and efficiency. It monitors a specific Google Sheet for new rows. When a new row is detected, it automatically takes the content (`post_text` and `image_url`) and posts it to your configured Twitter and Reddit accounts in parallel.


<img width="1775" height="697" alt="image" src="https://github.com/user-attachments/assets/b547c5dc-f94f-452e-b09c-f25c277ed7d3" />


##  Features

-   **Trigger-based Automation**: Runs automatically when you add new content to Google Sheets.
-   **Multi-platform Posting**: Cross-posts content to both Twitter (X) and Reddit simultaneously.
-   **Individual Processing**: Uses a loop to handle multiple new rows, ensuring each one becomes a unique post.
-   **Parallel Execution**: Posts to all platforms at the same time for faster execution.
-   **Easily Extensible**: Simple to add more social media platforms like LinkedIn, Facebook, etc.

##  Prerequisites

Before you begin, ensure you have the following:

-   An active **n8n instance** (self-hosted or cloud).
-   A **Google Account**.
-   A **Twitter (X) Developer Account** with API access.
-   A **Reddit Account**.

##  Setup Guide

Follow these steps to get the workflow up and running.

### 1. Prepare Your Google Sheet

Create a Google Sheet that will act as your content source. The first row must contain headers that match the ones used in the workflow.

-   Column `A`: `post_text`
-   Column `B`: `image_url`
<img width="1912" height="897" alt="image" src="https://github.com/user-attachments/assets/44171e41-8d4f-4cbe-af38-77cb7960ae35" />


### 2. Configure n8n Credentials

You need to add API credentials for each service to your n8n instance.

1.  In your n8n dashboard, go to **Credentials**.
2.  Click **Add credential**.
3.  Add credentials for the following services:
    * **Google Sheets** (Use OAuth2)
    * **Twitter (X)**
    * **Reddit**

### 3. Import and Configure the Workflow

1.  Download the workflow JSON file from this repository.
2.  In your n8n canvas, select **Import from File** and upload the JSON file.
3.  Configure the nodes as follows:

#### **A. Google Sheets Trigger**

This node starts the workflow.

-   **Credential**: Select your Google Sheets credential.
-   **Document**: Use the search to find and select the Google Sheet you created.
-   **Sheet**: Select the specific sheet name (e.g., `Sheet1`).
-   **Trigger On**: Set this to `Row Added`.



#### **B. Create Tweet (X Node)**

This node posts to your Twitter/X account.

-   **Credential**: Select your Twitter credential.
-   **Text**: Use an expression to map the text from the Google Sheet:
    ```
    {{ $json.post_text }}
    ```
-   **Media URLs**: Map the image URL from the sheet:
    ```
    {{ $json.image_url }}
    ```

#### **C. Create a post (Reddit Node)**

This node posts to Reddit.

-   **Credential**: Select your Reddit credential.
-   **Subreddit**: Enter the name of the subreddit you want to post to (e.g., `pics`, `testingground`).
-   **Title**: Map the post title. You can reuse the post text:
    ```
    {{ $json.post_text }}
    ```
-   **Content Type**: Select `Link` or `Image`.
-   **URL**: Map the image URL from the sheet:
    ```


### 4. Activate the Workflow

Once all nodes are configured correctly, save the workflow and **activate it** using the toggle at the top right of the screen.

## 🛠️ Usage

To post new content, simply add a new row to your Google Sheet. The workflow polls for changes every minute by default, so your posts should appear on Twitter and Reddit shortly after.

| post_text | image_url |
| :--- | :--- |
| `Every setback is a setup for a comeback. Keep moving forward.` | `https://images.unsplash.com/photo-1504384308090-c894fdcc538d` |
| `Our dreams don't work unless you do. Stay patient and trust your journey.` | `https://images.unsplash.com/photo-1496307742754-b4a456c4a2d` |

## 💡 Customization Ideas

-   **Add More Platforms**: Add new branches after the `Loop Over Items` node to post to LinkedIn, Facebook, Discord, or any other supported platform.
-   **Error Handling**: Set up an error workflow to send you a notification (e.g., via email or Slack) if a post fails.
-   **Scheduled Posting**: Change the trigger from "Row Added" to a **Cron** node to post content on a specific schedule (e.g., once daily). You would then need to add logic to read one row at a time and mark it as "posted" in the sheet.
