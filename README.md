# Auto_Jobs_Applier_AIHawk

> **Fork notice:** This is a fork of [feder-cr/Auto_Jobs_Applier_AIHawk](https://github.com/feder-cr/Auto_Jobs_Applier_AIHawk). For upstream community support, feature discussions, and contributions to the original project, visit the upstream repository. Issues specific to this fork can be opened [here](https://github.com/byoniq/Auto_Jobs_Applier_AIHawk/issues).

**Your AI-powered job search assistant. Automate LinkedIn Easy Apply applications, get personalized resume generation, and land your dream job faster.**

---

## Table of Contents

1. [Introduction](#introduction)
2. [Features](#features)
3. [Installation](#installation)
4. [Configuration](#configuration)
5. [Usage](#usage)
6. [Documentation](#documentation)
7. [Troubleshooting](#troubleshooting)
8. [License](#license)
9. [Disclaimer](#disclaimer)

---

## Introduction

Auto_Jobs_Applier_AIHawk automates the LinkedIn job application process. It scans job listings matching your criteria, auto-fills application forms using your profile, and uses an LLM (OpenAI, Claude, Gemini, or Ollama) to generate personalized responses to employer questions and dynamic tailored resumes.

**Confirmed working on:**
- Windows 10
- Ubuntu 22
- Python 3.10, 3.11.9 (64-bit), 3.12.5 (64-bit)

---

## Features

1. **Intelligent Job Search Automation**
   - Customizable search criteria (titles, locations, experience level, job type)
   - Continuous scanning for new openings
   - Smart filtering to exclude irrelevant listings

2. **Rapid and Efficient Application Submission**
   - One-click Easy Apply automation
   - Form auto-fill using your profile information
   - Automatic document attachment (resume, cover letter)

3. **AI-Powered Personalization**
   - Dynamic response generation for employer-specific questions
   - Tone and style matching to fit company culture
   - Keyword optimization for improved application relevance

4. **Volume Management with Quality**
   - Bulk application capability with quality controls
   - Detailed application tracking (success, failed, skipped)

5. **Intelligent Filtering and Blacklisting**
   - Company blacklist to avoid unwanted employers
   - Title filtering to focus on relevant positions

6. **Dynamic Resume Generation**
   - Automatically creates tailored resumes for each application based on job requirements

7. **Secure Data Handling**
   - Manages sensitive information locally via YAML files — nothing sent externally except to your chosen LLM API

---

## Installation

1. **Install Python** (3.10 or higher):
   - [Windows guide](https://www.geeksforgeeks.org/how-to-install-python-on-windows/)
   - [Linux guide](https://www.geeksforgeeks.org/how-to-install-python-on-linux/)
   - [macOS guide](https://www.geeksforgeeks.org/how-to-download-and-install-python-latest-version-on-macos-mac-os-x/)

2. **Install Google Chrome** (latest version, default location): [chrome.google.com](https://www.google.com/chrome)

3. **Clone this repository:**

   ```bash
   git clone https://github.com/byoniq/Auto_Jobs_Applier_AIHawk.git
   cd Auto_Jobs_Applier_AIHawk
   ```

4. **Create and activate a virtual environment:**

   ```bash
   python3 -m venv virtual
   # macOS/Linux:
   source virtual/bin/activate
   # Windows:
   .\virtual\Scripts\activate
   ```

5. **Install dependencies:**

   ```bash
   pip install -r requirements.txt
   ```

---

## Configuration

### 1. secrets.yaml

Contains sensitive credentials. **Never commit this file to version control.**

- `llm_api_key` — your OpenAI, Claude, or Gemini API key
  - OpenAI: [platform.openai.com/account/api-keys](https://platform.openai.com/account/api-keys)
  - Gemini: [Google AI Studio](https://ai.google.dev/gemini-api/docs/api-key)
  - Claude: [console.anthropic.com](https://console.anthropic.com/)

> **Note on OpenAI rate limits:** New accounts start on the Free tier (200 requests/day). Spending at least $5 on API credits automatically upgrades your tier. If you hit `429` errors, wait 12–24 hours after adding billing for the upgrade to propagate.

---

### 2. config.yaml

Defines your job search parameters and bot behavior.

| Field | Description |
|---|---|
| `remote` | `true` to include remote jobs |
| `experienceLevel` | Set desired levels to `true` |
| `jobTypes` | Set desired types to `true` |
| `date` | Set one time range to `true` |
| `positions` | List of job titles to search |
| `locations` | List of locations to search |
| `apply_once_at_company` | `True` to apply once per company |
| `distance` | Search radius in miles |
| `companyBlacklist` | Companies to exclude |
| `titleBlacklist` | Title keywords to avoid |

#### LLM Configuration

```yaml
llm_model_type: openai       # openai | ollama | claude | gemini
llm_model: gpt-4o            # model name
llm_api_url: https://api.openai.com/v1
```

Supported models:
- **openai**: `gpt-4o`, `gpt-4o-mini`
- **ollama**: `llama2`, `mistral:v0.3` (local — [Ollama setup guide](https://github.com/ollama/ollama))
- **claude**: any Anthropic model (`claude-sonnet-4-6`, etc.)
- **gemini**: any Gemini model

---

### 3. plain_text_resume.yaml

Your resume in structured YAML. Used for auto-filling forms and generating tailored resumes. Sections:

- `personal_information` — name, contact, LinkedIn, GitHub
- `education_details` — degrees, institutions, grades
- `experience_details` — roles, companies, responsibilities, skills
- `projects` — project names, descriptions, links
- `achievements` — awards and accomplishments
- `certifications` — professional certifications
- `languages` — spoken languages and proficiency
- `interests` — professional/personal interests
- `availability` — notice period
- `salary_expectations` — expected salary range (USD)
- `self_identification` — gender, pronouns, veteran/disability status
- `legal_authorization` — work authorization by country
- `work_preferences` — remote, relocation, assessments, background checks

See the `data_folder_example/` directory for fully populated example files showing the correct format for every field.

---

## Usage

1. Set your LinkedIn account language to **English**.

2. Ensure `data_folder/` contains:
   - `secrets.yaml`
   - `config.yaml`
   - `plain_text_resume.yaml`

3. Run the bot:

   ```bash
   # Dynamic resume generation (recommended — tailors resume per application)
   python main.py

   # Use a specific PDF resume for all applications
   python main.py --resume /path/to/your/resume.pdf

   # Collect job data only (no applications — useful for analytics)
   python main.py --collect
   ```

4. Output files are written to `output/`:
   - `success.json` — successful applications
   - `failed.json` — failed applications
   - `skipped.json` — skipped applications
   - `data.json` — collected job data (collect mode)
   - `open_ai_calls.json` — LLM API call log

> **Note:** `answers.json` in the project root stores LLM-generated answers. Search for `Select an option`, `0`, `Authorized`, and `how many years of` to review and correct answers.

---

## Documentation

- [Ollama & Gemini Setup Guide (PDF)](https://github.com/feder-cr/Auto_Jobs_Applier_AIHawk/blob/main/docs/guide_to_setup_ollama_and_gemini.pdf)
- [YAML Editing Guide (PDF)](https://github.com/feder-cr/Auto_Jobs_Applier_AIHawk/blob/main/docs/guide_yaml_sections.pdf)
- [Auto-start Guide (PDF)](https://github.com/feder-cr/Auto_Jobs_Applier_AIHawk/blob/main/docs/guide_to_autostart_aihawk.pdf)
- [Video Tutorial — Setup walkthrough](https://youtu.be/gdW9wogHEUM)
- [OpenAI API Documentation](https://platform.openai.com/docs/)
- [Workflow Diagrams](docs/workflow_diagrams.md)
- [Contribution Guidelines](CONTRIBUTING.md)

---

## Troubleshooting

### OpenAI Rate Limit (429 Error)

- Check billing at [platform.openai.com/account/billing](https://platform.openai.com/account/billing)
- Ensure a valid payment method is added
- ChatGPT Plus is **not** the same as API access — they require separate billing
- Free tier allows 200 RPD; spend $5+ to upgrade automatically
- Wait 12–24 hours after adding funds for tier upgrade to apply

### Easy Apply Button Not Found

- Verify you are logged into LinkedIn
- Confirm targeted job listings actually have the Easy Apply option
- Review `config.yaml` search parameters
- Increase page load wait time if elements are loading slowly

### Incorrect Application Answers

- Review `answers.json` in the project root
- Update the relevant entries with corrected values and re-run

### YAML Configuration Errors (`ScannerError`)

- Start from the example files in `data_folder_example/` and modify gradually
- Use a YAML validator to check syntax before running
- Check indentation — YAML is whitespace-sensitive; use spaces, not tabs

### Bot Logs In But Doesn't Apply

- Check for CAPTCHAs or LinkedIn security challenges requiring manual intervention
- Verify `config.yaml` search parameters are returning results
- Review console output for specific error messages

---

## License

MIT License — see [LICENSE](LICENSE) for details.

---

## Disclaimer

This tool is intended for personal use and educational purposes. The maintainers assume no responsibility for consequences arising from its use. Comply with LinkedIn's Terms of Service and all applicable laws and regulations. Automated job applications may carry risks including account restrictions. Use at your own discretion.
