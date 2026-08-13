# 🤖 AI Website Summarizer

An AI-powered website summarizer built with **Python, OpenAI API, Web Scraping, and Gradio**.

Enter any website URL, and the application extracts the webpage content and uses an LLM to generate a clean, concise summary.

## 🚀 Features

- 🌐 Accepts any website URL
- 🕷️ Extracts webpage content
- 🧠 Uses an LLM to summarize the content
- 📝 Generates a concise and readable summary
- 🎛️ Simple web interface using Gradio
- 🔐 API key loaded securely using `.env`

## 🏗️ How It Works

```text
User enters URL
       ↓
Web Scraper
       ↓
Extract webpage content
       ↓
OpenAI API
       ↓
LLM generates summary
       ↓
Gradio displays the result
