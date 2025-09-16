# Project: Proactive Procurement Copilot

This project is a functional no-code prototype of an enterprise multi-agent AI system built using **n8n**. It automates the initial, time-consuming stages of the vendor sourcing process, demonstrating a practical application of agentic AI principles in a business context.

## Table of Contents

-(#problem-statement)
-(#solution-overview)
-(#how-it-works-the-ai-agent-crew)
-(#technology-stack)
-(#setup-and-installation)
-(#how-to-run-the-workflow)
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


# Product Management Artifacts for the Proactive Procurement Copilot

This document contains the strategic product artifacts that guided the development of the Proactive Procurement Copilot prototype.

---

## 1. Product One-Pager

A one-pager is a concise, executive-level summary designed to secure buy-in and align stakeholders.

### Product One-Pager: Proactive Procurement Copilot

**Problem:**  
Enterprise procurement is a bottleneck. It is a slow, manual, and opaque process bogged down by repetitive research, complex compliance checks, and lengthy negotiations. This leads to extended cycle times, higher operational costs, and increased risk of non-compliance.

**Solution:**  
The Proactive Procurement Copilot is an intelligent, agentic system that transforms procurement from a reactive, manual function into a proactive, automated one. It employs a "crew" of specialized AI agents that collaborate to manage the end-to-end procurement workflow. A user simply delegates a high-level goal (e.g., *"Source a new cloud data warehouse provider"*), and the AI crew autonomously handles the research and initial vetting.

**Target Audience:**  
Procurement officers, legal and compliance teams, and finance departments in mid-to-large enterprises.

**Key Features (Multi-Agent Crew):**
- **Researcher Agent:** Scans internal databases and the public web to identify potential vendors.  
- **Compliance Agent:** Vets vendors against internal policies via AI-driven analysis.  
- **Summarizer Agent:** Generates concise vendor briefs and comparison matrices for human review.  

**Business Impact:**
- **Reduce Cycle Times:** Projecting a 50% reduction in the time from request to contract.  
- **Lower Operational Costs:** Automating up to 70% of manual research and vetting tasks.  
- **Improve Compliance:** Ensuring 100% of vendors are checked against compliance protocols automatically.  

---

## 2. Product Requirements Document (PRD) Snippet

The PRD is the single source of truth that details the product's purpose, features, and functionality for the development team.

### PRD: Proactive Procurement Copilot V1

#### 1. User Personas

- **Patricia, the Procurement Officer**  
  Overwhelmed with manual research tasks. Needs to find and vet vendors quickly to meet business deadlines. Values speed, accuracy, and compliance.  

- **Leo, the Legal Counsel**  
  Responsible for mitigating risk. Needs to ensure all new vendors meet strict compliance standards. Values thoroughness and auditability.  

#### 2. User Stories & Functional Requirements (MVP)

**Epic: Autonomous Vendor Sourcing**

- **PP-01 (Patricia):** As Patricia, I want to submit a natural language request for a new product or service so that I can initiate the sourcing process without filling out complex forms.  
- **PP-02 (System):** As the Researcher Agent, upon receiving a sourcing request, I need to execute searches against our internal vendor database (Google Sheet) and a public web search API to identify potential suppliers.  
- **PP-03 (System):** As the Compliance Agent, given a list of potential suppliers, I need to use an LLM to analyze their descriptions and determine if they meet a specified compliance rule (e.g., *"SOC 2 compliant"*).  
- **PP-04 (System):** As the Summarizer Agent, given the outputs from the previous agents, I need to generate a single-page summary document that lists the vendors, their key offerings, and their compliance status.  
- **PP-05 (Patricia):** As Patricia, I want to receive the summary document in my email within minutes of my initial request, so I can review the AI-generated findings.  

#### 3. Success Metrics

- **Primary:** *Time-to-Source* (Time from request to delivery of summary document). **Target:** < 5 minutes.  
- **Secondary:** *Human Effort Reduction* (Projected percentage of manual research time saved). **Target:** 70%.  

---

## Product Management Artifacts

This project was guided by standard product management practices to ensure it solves a real-world problem effectively. The key documents, including the **Product One-Pager** and **Product Requirements Document (PRD)**, can be found in the `PRODUCT_ARTIFACTS.md` file in this repository.
