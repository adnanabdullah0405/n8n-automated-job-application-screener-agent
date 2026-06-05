# 🤖 n8n AI Job Application Screener

> **Automated hiring pipeline — AI agent screens CVs, scores candidates, and sends interview invitations or rejections automatically. Zero manual CV reading.**

---

## 📊 Production Impact

| Metric | Result |
|---|---|
| CVs processed per run | Fully automated end-to-end |
| Manual screening time saved | ~15–20 min per application |
| AI match accuracy | GPT-4o agent with memory |
| Output | Structured score + auto email |

---

## 🧠 What This System Does

A candidate submits a job application form → their CV is automatically downloaded, extracted, and analyzed by an AI agent against the job requirements → the result is logged to Google Sheets → the system branches:

- ✅ **Strong match** → Interview invitation email sent automatically
- ❌ **Weak match** → Polite rejection email sent automatically

No HR person needs to open a single CV.

---

## 🔁 Pipeline Architecture

```
Google Sheets Trigger (anyUpdate)
        │
        ▼
Download File (Google Drive)
        │
        ▼
Extract from File (PDF)
        │
        ▼
AI Agent (GPT-4o + Memory + Tool Output Parser)
   └── Score candidate against job requirements
   └── Extract: skills, experience, education, fit score
        │
        ▼
JavaScript: Format & structure AI output
        │
        ▼
Append Row → Google Sheets (results log)
        │
        ▼
JavaScript: Evaluate fit threshold
        │
        ▼
IF condition (fit score ≥ threshold?)
   │                          │
   ▼ TRUE                     ▼ FALSE
Send Interview             Send Rejection
Invitation Email           Email (polite)
(Gmail)                    (Gmail)
```

---

## 🛠️ Tech Stack

| Component | Tool |
|---|---|
| Workflow Automation | n8n |
| Trigger | Google Sheets (anyUpdate) |
| File Storage | Google Drive |
| PDF Extraction | n8n Extract from File node |
| AI Agent | OpenAI GPT-4o |
| Memory | n8n AI Memory buffer |
| Output Parsing | Tool Output Parser |
| Logic | JavaScript (Code nodes) |
| Email | Gmail (Send Message + Wait) |
| Result Storage | Google Sheets |

---

## 🖼️ Workflow Screenshot

![n8n Job Application Screening Workflow](architecture/Job%20application%20automation.png)

---

## 🔄 Node-by-Node Breakdown

### 1. Google Sheets Trigger
- Fires on `anyUpdate` event
- Monitors the application intake sheet
- Each new row = one candidate submission

### 2. Download File
- Pulls the CV from Google Drive using the file link stored in Sheets
- Supports PDF, DOCX

### 3. Extract from File
- n8n's built-in PDF extractor
- Outputs raw text content of the CV

### 4. AI Agent (GPT-4o)
- Receives: CV text + job requirements (embedded in system prompt)
- Memory enabled — can reference previous context
- Tool Output Parser structures the response
- Returns: match score, reasoning, key strengths/gaps

### 5. Code Node (JavaScript)
- Formats AI output into clean structured fields
- Prepares data for Sheets append

### 6. Append Row → Google Sheets
- Logs: candidate name, email, score, AI reasoning, decision
- Full audit trail of every screening decision

### 7. Code Node 2 (JavaScript)
- Evaluates score against threshold (e.g., ≥ 7/10 = match)
- Sets `is_match` boolean for IF node

### 8. IF Node
- Routes to interview or rejection path

### 9. Gmail — Interview Invitation
- Personalized email with candidate name
- Waits for response (n8n human-in-the-loop pattern)

### 10. Gmail — Rejection
- Polite, professional rejection email
- Waits for delivery confirmation

---

## 📁 Repository Structure

```
n8n-ai-job-application-screener/
├── architecture/
│   └── Job application automation.png    # n8n workflow screenshot
├── README.md
```

---

## 🚀 How to Use

1. Import the workflow JSON into your n8n instance
2. Connect your Google Sheets, Google Drive, and Gmail credentials
3. Set your job requirements in the AI Agent system prompt
4. Set your fit score threshold in the JavaScript node
5. Activate the workflow — screening is fully automated from there

---

## 💡 Key Engineering Decisions

**Why n8n over custom code?**
Visual pipelines are faster to iterate, easier to hand off, and non-engineers can adjust email templates or score thresholds without touching code.

**Why AI Agent with Memory?**
For roles with complex requirements, the agent may need to reason across multiple sections of a CV — memory buffer enables this without re-sending full context each time.

**Why "Send and Wait" nodes?**
Allows a human recruiter to optionally intervene before the email sends, implementing a soft human-in-the-loop pattern without blocking the pipeline.

---

## 🔧 Skills Demonstrated

- n8n workflow automation (intermediate–advanced)
- AI Agent node with GPT-4o and memory
- PDF extraction and text processing
- Conditional branching logic
- Google Workspace integration (Sheets + Drive + Gmail)
- JavaScript inside n8n Code nodes
- Structured output parsing from LLMs
- Human-in-the-loop email patterns

---

## 📬 Connect

**Adnan Abdullah** — Agentic AI Engineer  
[LinkedIn](https://linkedin.com/in/adnan-abdullah-b6b49b1b6) • [Upwork](#) • [GitHub](https://github.com/adnanabdullah0405)
