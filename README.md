# ICP and Persona Analyzer

This repository contains an automated workflow that generates market research reports and audience profiles. These agents analyze uploaded customer lists (accounts and contacts) or manual inputs to generate comprehensive reports on Ideal Customer Profiles (ICPs) and detailed Buyer & User Personas. This workflow was developed using **N8N** as the workflow developer and **Google Gemini 3.1 Pro** LLMs as the intelligence layer.

---

## Overview

The workflow segments its analysis into two primary paths based on whether you are targeting an existing audience or a new one:

1. **Existing Audience Path:** Analyzes the statistical distribution of organizations, industries, company sizes, revenues, and job titles based on uploaded CSV/XLSX files.
2. **New Audience Path:** Generates persona profiles including KPIs, challenges, tech stacks, channels of engagement, adoption patterns, anticipated results, and questions for each stage of the customer journey.

---

## Workflow Structure

### 1. Data Input & Routing

* **Audience Analysis:** Select whether you're analyzing a "new" or "existing" audience, which routes the workflow to the successive nodes 
* **Buyer & User Personas:** A conditional branch determines whether the audience is a single target persona or a committee of buyers and users.

### 2. AI Analysis Engines

The workflow uses several **AI Agent nodes** powered by the **Google Gemini 3.1 Pro** model with specialized prompts:

* **Account Profile Agent:** Analyzes the  distributions of Companies, Industries (NAICS), Countries of Headquarters (U.N. standards), Annual Revenue, and Employee Count.
* **Contact Profile Agent:** Analyzes Job Titles, Company Names, Industries, and Countries of Origin from contact lists.
* **Persona Profile Agents:** Generates deep-dive reports including:
  * **Role & KPIs:** Standard responsibilities and measurable success metrics.
  * **Challenges:** Operational pain points.
  * **Tech Stack:** Common software used by the persona.
  * **Engagement Channels:** Specific publications, online forums, and professional associations.
  * **Adoption Patterns:** How is the persona adopting the technology? 
  * **Anticipated Results:** What results are they anticipating from usage?
  * **Common Questions:** Potential inquiries made by the target persona at each stage of the customer journey.

### 3. Data Processing & Delivery

* **Convert to File:** Transforms formatted text into `.md` (Markdown) files.
* **Gmail Integration:** Automatically sends the final reports to a designated email address.

---

## Requirements

* **n8n** (Self-hosted or Cloud)
* **Google Gemini API Key:** Required for the AI Language Model nodes.
* **Gmail OAuth2 Credentials:** Required for the automated email delivery.

---

## Input Data Requirements

For **Existing Audience** analysis, your uploaded files should ideally contain:

| Account List Fields | Contact List Fields |
| --- | --- |
| Company Name | Job Title |
| Industry | Company Name |
| Country of HQ | Industry |
| Annual Revenue | Country of Location |
| Number of Employees |  |

---

## How to Use

1. **Import the JSON:** Download the `ICP & Persona Analysis.json` file and import it into your n8n instance.
2. **Configure Credentials:**
* Set up the **Google Gemini(PaLM) API** in the Gemini Chat nodes.
* Connect your **Gmail account** in the Gmail nodes.
3. **Set Recipient:** Update the `sendTo` field in the Gmail nodes to your preferred email address.
4. **Execute:** Open the "Audience analysis" form URL, fill in your details, and wait for the reports to arrive in your inbox.

---

## Output Examples

The workflow generates several Markdown reports, including:

* `account_profile.md`
* `contact_profile.md`
* `target_persona_profile.md`
* `buyer_persona_profile.md`
* `user_persona_profile.md`
