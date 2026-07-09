# ICP & Persona Analyzer

The **ICP & Persona Analyzer** generates market-research reports and audience profiles for B2B marketers. It analyzes uploaded account and contact lists — or short descriptions of your target buyers and users — to produce Ideal Customer Profile (ICP) distributions and detailed Buyer & User Personas. Built on **n8n**, the latest **Gemini Pro** and **Gemini Flash** models, and **Gmail**; results arrive in your inbox as Markdown files.

---

## Core Features

The workflow segments its analysis into two paths based on whether you are targeting an existing audience or a new one:

- **Existing Audience Path:** analyzes the statistical distribution of organizations, industries, company sizes, revenues, and job titles from uploaded CSV/XLSX files.
- **New Audience Path:** generates persona profiles including KPIs, challenges, tech stacks, engagement channels, adoption patterns, anticipated results, and questions for each stage of the customer journey.

Its specialized AI agents (latest Gemini Pro for analysis, latest Gemini Flash for persona drafting) include:

- **Account Profile Agent:** distributions of Companies, Industries (NAICS), Countries of Headquarters (U.N. standards), Annual Revenue, and Employee Count.
- **Contact Profile Agent:** Job Titles, Company Names, Industries, and Countries of Origin from contact lists.
- **Persona Profile Agents:** deep-dive reports covering **Role & KPIs** (responsibilities and success metrics), **Challenges** (operational pain points), **Tech Stack** (common software), **Engagement Channels** (publications, forums, associations), **Adoption Patterns** (how the persona adopts the technology), **Anticipated Results** (what they expect from usage), and **Common Questions** across each stage of the customer journey.

---

## Workflow Architecture

The entry form routes to one of two branches via a Switch.

**Audience analysis:**
`Upload customer details → Company & contact lists (Switch) → Analyze document / Analyze document1 (latest Gemini Pro) → Convert to File / Convert to File1 → Merge Profiles → Send Audience Analysis`

**Persona profiles:**
`Is the buyer the user? → Yes/No (IF) → Target persona details / Buyer & persona details → Basic LLM Chain / Basic LLM Chain1 (latest Gemini Flash) → Convert to File2 / Convert to File3 → Send Target Persona Profile / Send Buyer & User Persona Profiles`

In this version both audience-analysis upload fields are **required**, and `Merge Profiles` combines the two profile files onto one item so a single email carries both attachments.

---

## Technical Specifications

| | |
| --- | --- |
| **Platform** | n8n (Cloud or self-hosted) |
| **Models** | `gemini-pro-latest` (document analysis), `gemini-flash-latest` (persona drafting) |
| **Input formats** | `.csv`, `.xlsx` (audience analysis); form text (persona profiles) |
| **Output format** | Markdown (`.md`) |
| **Integrations** | Google Gemini (PaLM) API, Gmail OAuth2 |

---

## Requirements

- **n8n** (Cloud or self-hosted) with the LangChain nodes package (`@n8n/n8n-nodes-langchain`)
- **Google Gemini API key** — for the AI language-model nodes
- **Gmail OAuth2 credentials** — for automated email delivery

---

## Inputs

- **Entry:** "Are you analyzing your audience or generating persona profiles?"
- **Persona profiles:** whether the buyer is also the user, then target/buyer/user job titles, industries, countries, and the product category being evaluated.

For **audience analysis** (both files required in this version), your uploaded lists should contain:

| Account List Fields | Contact List Fields |
| --- | --- |
| Company Name | Job Title |
| Industry | Company Name |
| Country of HQ | Industry |
| Annual Revenue | Country of Location |
| Number of Employees | — |

---

## Outputs

- **Audience analysis** (both attached to one email): `account_profile.md`, `contact_profile.md` — top-10 distributions per category with the remainder grouped as "Other," NAICS industry naming, U.N. country naming, standardized revenue/employee bands.
- **Persona profiles:** `target_persona_profile.md` (when the buyer is also the user) or `buyer_and_user_persona_profile.md` (when they differ) — role, KPIs, challenges, tech stack, engagement channels, adoption patterns, anticipated results, and journey questions.

---

## Setting Up Credentials

After import, n8n will show the Gemini and Gmail nodes as needing a credential. Set each up once, then select it on the relevant nodes.

### Google Gemini

Nodes that use it: **Analyze document**, **Analyze document1**, **Google Gemini Chat Model12**, **Google Gemini Chat Model13**.

1. Get an API key from [Google AI Studio](https://aistudio.google.com/app/apikey): sign in and click **Get API key → Create API key**. Copy it.
2. In n8n: **Credentials → Add credential → search "Google Gemini(PaLM) API"**.
3. Paste the key into the **API Key** field and **Save** (e.g. name it "Google Gemini API account").
4. Open each of the four Gemini nodes above and select the credential in the **Credential to connect with** dropdown.

### Gmail

Nodes that use it: **Send Audience Analysis**, **Send Target Persona Profile**, **Send Buyer & User Persona Profiles**.

1. In n8n: **Credentials → Add credential → search "Gmail OAuth2 API"**.
2. Connect your account:
   - **n8n Cloud:** click **Sign in with Google** and authorize the Gmail account to send from.
   - **Self-hosted:** create a Google Cloud OAuth client — in [Google Cloud Console](https://console.cloud.google.com/), pick/create a project → enable the **Gmail API** → **APIs & Services → Credentials → Create OAuth client ID (Web application)** → add n8n's **OAuth Redirect URL** (shown on the credential screen) to the authorized redirect URIs → paste the **Client ID** and **Client Secret** into n8n → **Sign in with Google** and authorize.
3. **Save**, then select this credential on all three Gmail nodes above.

---

## Set the Recipient Email

All three Gmail nodes ship with `sendTo = your-email@example.com`. Open **Send Audience Analysis**, **Send Target Persona Profile**, and **Send Buyer & User Persona Profiles** and change the **To** field to your address.

---

## Import and Run

1. In n8n: **Workflows → Import from File →** choose `ICP & Persona Analyzer.json`.
2. Configure the Gemini and Gmail credentials (above) and set the recipient email.
3. The workflow imports **inactive**. Open the **Audience analysis** form trigger for the form URL, fill in your details to test a run, then toggle the workflow **Active**.

---

## Notes

- Both audience-analysis upload fields are **required** in this version, and `Merge Profiles` combines the two profile files onto one item so one email carries both attachments. Supporting single-list runs would require a different design (conditional attachments).
- Models: the latest **Gemini Pro** (`gemini-pro-latest`) for document analysis and the latest **Gemini Flash** (`gemini-flash-latest`) for persona drafting.
- Keep your Gemini API key private; don't commit a filled-in copy of this workflow to a public repo.
