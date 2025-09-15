# Project: Proactive Procurement Copilot

This project is a functional no-code prototype of an enterprise multi-agent AI system built using **n8n**. It automates the initial, time-consuming stages of the vendor sourcing process, demonstrating a practical application of agentic AI principles in a business context.

## Table of Contents

-#problem-statement
-#solution-overview
-#how-it-works-the-ai-agent-crew
-#technology-stack
-#setup-and-installation
-#how-to-run-the-workflow
- [Product Management Artifacts](#product-management-artifacts)

## Problem Statement

Enterprise procurement is a major operational bottleneck. Procurement officers spend up to 70% of their time on manual, low-value tasks like researching potential vendors, cross-referencing internal databases, and performing initial compliance checks. This manual process is slow, prone to human error, and increases operational costs.

## Solution Overview

The Proactive Procurement Copilot is an automated workflow that acts as an intelligent assistant. A user provides a simple, natural-language request (e.g., "Find me vendors for a cloud data warehouse that is SOC 2 compliant"), and the system autonomously performs the initial research and analysis, delivering a concise summary report directly to the user's email.

This prototype is designed to prove the core value proposition: reducing manual effort by over 70% and cutting the initial sourcing time from hours or days to just a few minutes.

## How It Works: The AI Agent Crew

The workflow simulates a multi-agent system where each "agent" is a specialized node in the n8n canvas:

1.  **Orchestrator (Start Node):** The workflow is manually triggered by a user who inputs their sourcing request.
2.  **Researcher Agent (Google Sheets & HTTP Request Nodes):** This agent works in parallel:
    *   It queries an "internal database" (a Google Sheet) to find existing vendors.
    *   It simultaneously performs a web search using the Serper API to discover new potential vendors based on the user's request.
3.  **Compliance Agent (OpenAI Node):** This agent receives the combined list of vendors from the Researcher. It uses a powerful LLM (GPT-4o) to analyze the description of each vendor and determine if it meets a specific compliance requirement (in this case, being "SOC 2 compliant"). It outputs a structured JSON list of its findings.
4.  **Summarizer Agent (OpenAI Node):** This agent takes the structured JSON from the Compliance Agent and generates a clean, human-readable summary report in Markdown format, suitable for an executive review.
5.  **Communications Agent (Send Email Node):** This final agent delivers the formatted report to the user's email address.

## Technology Stack

*   **Workflow Automation:** [n8n.io](https://n8n.io/) (Cloud or Self-Hosted)
*   **Internal Database:** Google Sheets
*   **Web Search Tool:**(https://serper.dev/)
*   **AI Reasoning Engine:** OpenAI API (GPT-4o)

## Setup and Installation

**1. Prerequisites:**

*   An **n8n** account ([cloud-hosted](https://n8n.io/) is recommended for ease of use).
*   A **Google Account** with a new Google Sheet set up as your vendor database.
*   An **OpenAI API Key**.
*   A **Serper API Key**.

**2. Configure External Services:**

*   **Google Sheet:** Create a sheet with columns like `VendorName`, `Category`, `Description`, and `InternalStatus`. Add some sample data.
*   **API Keys:** Make sure you have your API keys from OpenAI and Serper ready.

**3. Set up n8n:**

*   Log in to your n8n account and create a new, blank workflow.
*   Click the three dots (`...`) in the top right corner, select **Import from file**, and upload the `workflow.json` from this repository.
*   The complete workflow will appear on your canvas.

**4. Configure Credentials in n8n:**
You must connect your own accounts to the workflow nodes. Click on each of the following nodes and add your credentials:

*   **Researcher Agent: Internal DB (Google Sheets Node):**
    *   In the "Credential" field, click "Create New" and follow the steps to connect your Google account.
    *   Once connected, select your vendor database spreadsheet from the "Sheet ID" dropdown.
*   **Researcher Agent: Web Search (HTTP Request Node):**
    *   In the "Authentication" field, select `Header Auth`.
    *   Click "Create New" for the credential.
    *   Set the **Name** to `X-API-KEY` and the **Value** to your Serper API key.
*   **Compliance Agent & Summarizer Agent (OpenAI Nodes):**
    *   For both nodes, click on the "Credential" field, select "Create New", and add your OpenAI API key.
*   **Send Final Report (Send Email Node):**
    *   Click on the "Credential" field, select "Create New", and configure your email provider (e.g., Gmail via OAuth2).
    *   Change the `To` address in the parameters to your own email.

## How to Run the Workflow

1.  Click on the **Start** node.
2.  Click **Test step**.
3.  In the pop-up, enter your procurement request (e.g., `Find me three potential vendors for enterprise-grade laptops with SOC 2 compliance`).
4.  Click **OK**.
5.  The workflow will now execute step-by-step. You can click on each node to see the input and output data.
6.  Within a minute, you should receive an email with the final procurement report.

## Product Management Artifacts

This project was guided by standard product management practices to ensure it solves a real-world problem effectively. The key documents, including the **Product One-Pager** and **Product Requirements Document (PRD)**, can be found in the `PRODUCT_ARTIFACTS.md` file in this repository.
