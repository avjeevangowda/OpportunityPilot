# OpportunityPilot

An AI agent built with **n8n** that reads placement and internship emails, extracts the deadline and eligibility, decides whether a student should apply, and prepares the application, so no opportunity is missed.

**Track:** AI Automation with n8n (PS22)
**Team:** Code hackers
**Members:** Jeevan gowda A V, Hruthwick M S, Shuchivrath S M,Dhanush gowda A M



## Problem

Internship and placement opportunities arrive across emails and group messages. Deadlines and eligibility are buried in long text, so students apply late, miss them, or spend hours tracking them by hand.



## Solution

OpportunityPilot is an n8n workflow that acts on each new placement email:

1. **Gmail trigger** picks up new emails labelled `Placements`.
2. **AI extraction (Gemini)** pulls out company, role, deadline, eligibility and link, and checks whether the email is a real opportunity.
3. **Filter** drops emails that are not opportunities (for example, library reminders).
4. **Match score (Gemini)** compares the opportunity with the student profile and returns `Apply` or `Skip` with a short reason.
5. **Google Sheets** logs the opportunity and the decision.
6. **Google Calendar** creates a reminder on the deadline.
7. **Draft email (Gemini)** writes a short application email.
8. **Gmail draft** is saved with the resume attached. Nothing is sent automatically.
9. **Telegram** sends an alert with the decision, reason and draft.
10. **Daily digest** (second workflow) sends a summary of deadlines due in the next 7 days.



### What makes it more than simple automation

* It **decides** (Apply or Skip) and **explains why**.
* It uses **multi-step reasoning**: classify, extract, score, then act.
* A **human stays in control**: drafts are saved for review, never sent on their own.



## Architecture

!\[Workflow](docs/workflow.png)

!\[Data flow diagram](docs/dfd.png)

!\[Plan mind map](docs/mindmap.png)



## Tech stack

|Tool|Purpose|
|-|-|
|n8n|Workflow engine and AI orchestration|
|Google Gemini|Extraction, match score, email drafting|
|Gmail|Email trigger and application draft|
|Google Drive|Stores the resume that is attached|
|Google Sheets|Opportunity tracker|
|Google Calendar|Deadline reminders|
|Telegram Bot|Alerts and daily digest|

## 

## Repository structure

```
OpportunityPilot/
├── README.md
├── .gitignore
├── workflow/
│   ├── opportunitypilot.json    main workflow
│   ├── digest.json              daily digest workflow
│   └── README.md                import instructions
├── prompts/
│   ├── extraction.txt
│   ├── match-score.txt
│   └── draft-email.txt
├── samples/
│   ├── sample-emails.md         test emails (fictional companies)
│   └── resume-sample.pdf        sample resume used in the demo
└── docs/
    ├── workflow.png
    ├── dfd.png
    └── mindmap.png


## 

## How to run it

1. **Start n8n** (Docker):

&#x20;  docker run -it --rm --name n8n -p 5678:5678 -v n8n\_data:/home/node/.n8n docker.n8n.io/n8nio/n8n  

   Open `http://localhost:5678`.

2. **Import the workflows:** in n8n, use **Import from file** for `workflow/opportunitypilot.json` and `workflow/digest.json`.
3. **Create your own credentials** in n8n (they are not included in this repo):

   * **Google (Gmail, Sheets, Calendar, Drive):** create an OAuth client in Google Cloud, with redirect URI `http://localhost:5678/rest/oauth2-credential/callback`. Enable the Gmail, Sheets, Calendar and Drive APIs.
   * **Gemini:** an API key from Google AI Studio.
   * **Telegram:** a bot token from @BotFather, and your chat ID.
4. **Prepare Google:**

   * Gmail label `Placements`.
   * A Google Sheet named `Opportunities` with columns: `company, role, deadline, eligibility, link, decision, reason`.
   * Upload your resume PDF to Google Drive and select it in the Drive node.
5. **Edit the student profile** in the match-score node (skills, year, interests).
6. **Test** with the emails in `samples/sample-emails.md`, then publish both workflows.

## 

## Notes and limitations

* The workflow runs **locally**, so it works only while n8n is running. It can be moved to n8n Cloud.
* Telegram approve/disapprove buttons need a public URL, which `localhost` does not provide. For that reason, drafts are saved in Gmail for the student to review and send.
* The Gmail trigger processes **new** labelled emails only.
* Company names in the samples are fictional.
* **No credentials, tokens or API keys are stored in this repository.**

## 

## Future scope

* WhatsApp group integration
* Approve and send buttons on n8n Cloud
* Auto-tailored resume for each role
* Multi-language support
* College-wide placement dashboard



