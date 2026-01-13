# Mimesis | AI Text Humanizer

[cite_start]**Mimesis** is an end-to-end text humanization pipeline designed to replicate a user's unique linguistic fingerprint[cite: 5]. [cite_start]Unlike generic AI rewriters, this system is trained exclusively on personal exported data from Telegram, capturing authentic informal, formal, and academic communication styles[cite: 5].

## Project Overview
[cite_start]The core objective of Mimesis is to bypass AI detection by injecting "human imperfections"—specifically high perplexity and burstiness—into generated text. [cite_start]By analyzing personal archives, the engine mimics specific structural habits, punctuation densities, and even the subtle "Slavic syntax" of a non-native English speaker to ensure 0% AI detection scores.

## Key Features
* [cite_start]**Style Mimicry**: Trained on real-world personal chat data across three distinct modes: Chatting, Formal, and Academic[cite: 1, 5].
* [cite_start]**Anti-AI Logic**: Implements a "Negative Style Bank" and structural disruption rules to remove common AI-isms like "Furthermore," "Moreover," or "Delve".
* [cite_start]**Modular Backend**: Currently powered by the OpenAI GPT-4o API but architected to be easily replaced with local LLMs like Llama or Mistral[cite: 5].
* [cite_start]**Netflix-Style UI**: A cinematic, high-contrast dark mode interface featuring real-time transformation, creativity sliders, and tap-to-copy functionality.
* [cite_start]**Post-Processing Scrambler**: A custom logic layer that randomly replaces "too perfect" AI transitions with messy, human alternatives.

## Project Structure
[cite_start]The project follows a modular pipeline from raw data ingestion to final text generation[cite: 2]:

```text
humanizer_project/
├── data/
│   ├── raw/
│   │   └── result.json              # Raw source file (Source of Truth - DO NOT MODIFY)
│   ├── intermediate/                # Data processing pipeline stages
│   │   ├── 01_raw_df.csv            # Initial DataFrame (all messages)
│   │   ├── 02_cleaned.csv           # Filtered and preprocessed data (B1, B2)
│   │   └── 03_labeled.csv           # Stylistic segmentation and labeling (C)
│   └── gold/
│       ├── corpus_en.json           # Generation corpus (English target style)
│       └── corpus_ru.json           # Reference corpus (Russian structural patterns)
│
├── src/                             # Source code
│   ├── 1_ingestion.py               # JSON to Pandas parsing (A1)
│   ├── 2_cleaning.py                # Data cleaning and filtering (B1, B2)
│   ├── 3_profiling.py               # Style analysis and metrics (E, F, G, H)
│   └── generation.py                # LLM interaction and Humanization logic (L, M)
│
├── notebooks/                       # Sandbox for research and testing
│   ├── 01_data_exploration.ipynb    # Initial data visualization and inspection
│   └── 02_prompt_engineering.ipynb  # Prompt testing and iterative refinement
│
├── static/                          # Web assets (Frontend)
│   └── css/style.css                # Netflix-style styling
│
├── templates/
│   └── index.html                   # Main UI template (Self-written)
│
├── .env                             # Environment variables (API Keys)
├── requirements.txt                 # Dependency list (pandas, nltk, openai, flask)
└── main.py                          # Application entry point (Flask Server / Pipeline CLI)
```

## Technical Implementation
### Humanization Logic
The system uses a complex system prompt that enforces strict style metrics derived from the user's `style_profile.json`:
- Burstiness: Mixes sentence lengths aggressively.
- Slavic Syntax: Encourages direct, "heavy" sentences and subtle article omissions to reflect non-native proficiency.
- Logit Bias: Hard-bans common AI tokens within the API call to force more creative word choices.
