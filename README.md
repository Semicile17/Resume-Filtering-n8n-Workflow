

# 📄 **Resume Filtering Automation using n8n (AI + RAG + Google Drive)**

This project is an **AI-powered resume filtering system built in n8n**.
It automatically processes resumes uploaded to a Google Drive folder, evaluates them against a Job Description using RAG (Retrieval-Augmented Generation), extracts candidate information, computes a match score, and sends shortlisted candidates an email.

---

## 🚀 **Features**

### ✅ **Automatic Resume Ingestion**

* Watches a Google Drive folder.
* Runs whenever a **new resume PDF** is added.

### ✅ **AI-Powered Resume Evaluation**

* Extracts text from resumes.
* Retrieves Job Description from a Vector Database (Pinecone/Supabase).
* Compares resume vs JD using semantic AI (Groq/Ollama/OpenAI/Gemini supported).
* Returns:

  * Full name
  * Email
  * Phone
  * Skills matched
  * Skills missing
  * Fit score (0–100)
  * Match flag (true/false)

### ✅ **RAG (Retrieval-Augmented Generation)**

* JD is embedded using an Embeddings model (Gemini/Groq/Ollama).
* Stored in Pinecone/Supabase.
* AI Agent retrieves it dynamically.

### ✅ **Automated Shortlist Workflow**

If candidate matches:

* Add to Google Sheet
* Send HTML interview invitation email
* Store metadata
* Continue to next resume

### ❌ If candidate does NOT match:

* Skip
* Move on to next resume

### 🔁 **Fully Hands-Off**

Upload resumes → AI filters candidates → Emails go out automatically.

---

## 📂 **Repository Structure**

```
n8n-resume-filtering/
│
├── workflow.json               # Exported n8n workflow
├── README.md                   # Documentation
├── env.example                 # Template for environment variables
│
└── samples/
    ├── Sample_Resume.pdf       # Example resume
    └── Sample_JD.pdf           # Example job description
```

---

## 🧠 **How It Works (Architecture)**

### 1. **Job Description Setup**

You upload the JD PDF manually → n8n extracts text → creates embeddings → stores in vector DB.

### 2. **Resume Trigger**

When a resume is uploaded to Google Drive → Trigger fires.

### 3. **Processing Pipeline**

```
Fetch Resume → Download → Extract PDF Text → AI Agent (JSON Output) 
→ Structured Output Parser → IF Match → Google Sheets + Email
```

### 4. **AI Agent Tasks**

* Extract candidate information
* Fetch JD via RAG
* Compute match score
* Produce clean JSON

### 5. **Shortlisted Candidate**

Receives a professional HTML email with a scheduling link.

---

## 🧪 **Technologies Used**

### **n8n Modules**

* Google Drive Trigger
* Download File
* PDF Extract
* AI Agent
* Vector Store Node (Pinecone/Supabase)
* Structured Output Parser
* Google Sheets
* Email Node

### **Models**

* **Groq (Llama-3 8B)**
* OR local **Ollama**
* OR cloud:

  * Gemini
  * OpenAI
  * Mistral

### **Vector Database**

* Pinecone
* OR Supabase Vector Store

---

## 🧰 **Setup Instructions**

### 1️⃣ Clone this repo

```bash
git clone https://github.com/YOUR_GITHUB_USERNAME/n8n-resume-filtering.git
cd n8n-resume-filtering
```

### 2️⃣ Import the Workflow into n8n

Inside n8n →

```
Workflows → Import → Select workflow.json
```

### 3️⃣ Add Required Credentials

Inside n8n → Credentials:

| Service                           | Needed For                     |
| --------------------------------- | ------------------------------ |
| **Google Drive**                  | Resume ingestion               |
| **Google Sheets**                 | Logging shortlisted candidates |
| **Vector DB (Pinecone/Supabase)** | Store JD embeddings            |
| **AI Model (Groq/LLM)**           | Evaluate resumes               |
| **SMTP / Gmail**                  | Send shortlisted email         |

### 4️⃣ Fill Environment Variables

Use `env.example`:

```
GOOGLE_DRIVE_CLIENT_ID=
GOOGLE_DRIVE_CLIENT_SECRET=
GROQ_API_KEY=
PINECONE_API_KEY=
EMAIL_HOST=
EMAIL_USER=
EMAIL_PASS=
```

---

## 📧 **Email Templates**

Shortlisted Email:

* Located inside the workflow
* Customizable HTML
* Includes placeholders:

  * `{{$json.full_name}}`
  * `{{$json.email}}`
  * `{{$json.score}}`
  * `{{$json.interview_link}}`

You can modify it under the “Send Email” node.

---

## 🧪 **Testing**

Upload any sample resume into your connected **Google Drive → RESUMES/** folder.

n8n will:

1. Trigger
2. Process the resume
3. Evaluate
4. Shortlist or skip

---

## 🛠️ **Troubleshooting**

### ❗ The old resume keeps triggering

Enable **Deduplication** in Google Drive Trigger.

### ❗ Output Parser error

Make sure AI Agent returns **valid JSON only**, no markdown.

### ❗ Rate Limits from Gemini

Switch to **Groq** or **Ollama**.

---

## 🧑‍💻 **Author**

**Rohit Mahant**
GitHub: [https://github.com/Semicile17](https://github.com/Semicile17)

---

## ⭐ **Contributions**

PRs welcome!
You can add:

* Multi-resume ranking
* Applicant dashboard
* Bulk JD mode
* ATS integration

---

## 📜 License

MIT License — Free to use, modify, and distribute.


