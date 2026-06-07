# CUD-Offerings-Scraper

> An AI-powered web scraper that autonomously navigates the CUD Student Portal, extracts course offering data, and lets you query it using natural language — all through a Streamlit UI.

---

## Overview

CUD Offerings Scraper uses the [Browser Use](https://browser-use.com/) AI agent framework to log into the Canadian University Dubai student portal, filter courses by the SEAST division, and extract full course details across all pages. The scraped data is saved to a CSV file and can then be queried in plain English using either Google Gemini or a local Ollama model.

Built as a group project for BCS 206 – Information Structures at Canadian University Dubai.

---

## Features

- **Autonomous browser navigation** — logs in, applies filters, and paginates through course listings without manual input
- **Structured data extraction** — pulls Course Code, Name, Credits, Instructor, Room, Days, Start/End Time, and Max/Total Enrollment
- **CSV export** — saves all extracted data with timestamped filenames to avoid overwriting
- **Dual LLM support** — choose between Google Gemini (cloud) or Ollama/Mistral (local) at runtime
- **Natural language querying** — ask questions about the scraped data in plain English (e.g. *"Show me all courses offered on Mondays"*)
- **Streamlit UI** — no terminal interaction needed; everything runs through a web interface

---

## Tech Stack

| Layer | Technology |
|---|---|
| UI | Streamlit |
| Browser Automation | Browser Use (AI Agent) |
| LLM (Cloud) | Google Gemini 2.0 Flash (`langchain-google-genai`) |
| LLM (Local) | Ollama / Mistral (`langchain-ollama`) |
| Data Validation | Pydantic |
| Data Storage | CSV |
| Config | python-dotenv |

---

## Project Structure

```
CUD-Offerings-Scraper/
├── AIScraper.py          # Main application
├── .env                  # API keys (not committed — see setup)
├── .env.example          # Template for environment variables
├── requirements.txt      # Python dependencies
└── README.md
```

---

## Setup

### Prerequisites

- Python 3.10+
- [Ollama](https://ollama.com/) installed locally (if using local LLM)
- A Google Gemini API key (free at [aistudio.google.com](https://aistudio.google.com))

### Installation

```bash
git clone https://github.com/your-username/CUD-Offerings-Scraper.git
cd CUD-Offerings-Scraper
pip install -r requirements.txt
```

### Environment Variables

Create a `.env` file in the root directory:

```env
GEMINI_API_KEY=your_gemini_api_key_here
```

> **Never commit your `.env` file.** It is listed in `.gitignore`.

### Running the App

```bash
streamlit run AIScraper.py
```

---

## Usage

1. Enter your CUD portal **username** and **password**
2. Enter the **term** (e.g. `sp23-24`)
3. Specify a **CSV filename** and a **save path** on your machine
4. Choose your **LLM** — Gemini or Ollama
5. Click **Submit** — the agent will log in, apply the SEAST filter, and scrape all pages
6. Once complete, use the query box to ask questions about the extracted data in plain English

---

## Data Structures Used

- **Pydantic `BaseModel`** — used for the `Course` class to validate and structure scraped records before writing to CSV; also used via `SecretStr` to handle credentials securely
- **Dictionary** — stores form input values (`form_values`), browser agent credentials (`sensitive_data`), and initial browser actions (`initial_actions`)
- **List** — used in `initial_actions` and CSV row writing via `writer.writerow`
- **CSV file** — functions as a flat list-of-lists (matrix) for persistent structured storage, readable in Excel or any spreadsheet tool
- **Environment Variables** — loaded via `python-dotenv` for secure, flexible API key management

---

## Example CSV Output

```
Course Code,Course Name,Credits,Instructor,Room,Days,Start Time,End Time,Max Enrollment,Total Enrollment
ACT112Lec1,Principles Of Accounting 1,3,Mostafa Hemdan,MGT-202,M,9:00:00 AM,11:59:00 AM,50,52
BCS206Lec1,Information Structures,3,Dr. Said Elnaffar,SEAST-101,TR,9:00:00 AM,10:29:00 AM,35,30
```

---

## Team

| Student | ID |
|---|---|
| Maryam Saad Khairullah Alyaseen | 20220002536 |
| Yasmin Issa | 20230003798 |
| Fatima Bey | 20220001636 |
| Leanne Jessica Rodrigo | 20210001983 |
| Aysha Thansim Ejaz | 20220002458 |

---

## Known Limitations

- Scraping speed depends on the chosen LLM's response time and browser rendering
- Gemini free tier has rate limits that may affect large scraping runs
- The portal's session may time out on very long runs; re-running from the last page is a workaround
- Ollama requires a locally running Mistral model (`ollama pull mistral` before first use)

---

## License

This project was developed for academic purposes at Canadian University Dubai. Not intended for commercial use.
