# 🎥  MeetPulse – AI Video Assistant

An AI-powered meeting assistant that converts audio and video recordings into structured meeting summaries, action items, decisions, and follow-up questions. It supports English transcription using local OpenAI Whisper and Hindi/Hinglish transcription using Sarvam AI, with Retrieval-Augmented Generation (RAG) for asking questions about meeting content.

## 📑 Table of Contents

1. [Brief Summary](#-brief-summary)
2. [Overview](#-overview)
3. [Problem Statement](#-problem-statement)
4. [Input Sources](#-input-sources)
5. [Tools and Technologies](#️-tools-and-technologies)
6. [Methods](#️-methods)
7. [Audio Processing](#-audio-processing)
8. [Speech Transcription](#️-speech-transcription)
9. [Meeting Summarization](#-meeting-summarization)
10. [Information Extraction](#-information-extraction)
11. [Retrieval-Augmented Generation (RAG)](#-retrieval-augmented-generation-rag)
12. [User Interface](#️-user-interface)
13. [Key Features](#-key-features)
14. [Dashboard / Model / Output](#-dashboard--model--output)
15. [How to Run](#-how-to-run)
16. [Limitations](#️-limitations)
17. [Future Work](#-future-work)
18. [Author & Contact](#-author--contact)

## 📌 Brief Summary

AI Video Assistant is a meeting transcription and analysis platform designed to reduce the time required to review long recordings. It transcribes meetings, generates concise summaries, extracts action items and decisions, and enables users to ask questions about the transcript through a RAG-based chat interface.

## 📖 Overview

The application accepts YouTube links and local audio or video files. It extracts audio, converts it into a suitable format, and processes it in manageable chunks.

Depending on the selected language, transcription is performed using local OpenAI Whisper or Sarvam AI. Mistral AI generates summaries and meeting insights, while ChromaDB and Hugging Face embeddings support contextual question answering.

## 🎯 Problem Statement

Meetings often contain important information spread across lengthy recordings. Manually reviewing recordings and preparing notes can be time-consuming.

This project automates transcription, summarization, action-item extraction, and transcript-based question answering in one application.

## 📂 Input Sources

- YouTube video URLs
- Local audio files
- Local video files

## 🛠️ Tools and Technologies

| Technology | Purpose |
|---|---|
| Python | Core programming language |
| Streamlit | Interactive user interface |
| OpenAI Whisper | Local English speech transcription |
| Sarvam AI | Hindi/Hinglish speech transcription |
| Mistral AI | Meeting summarization and information extraction |
| LangChain LCEL | Language-model chain orchestration |
| ChromaDB | Persistent vector database for transcript retrieval |
| Hugging Face Embeddings | Local sentence embeddings using `all-MiniLM-L6-v2` |
| yt-dlp | Audio extraction from supported online videos |
| FFmpeg | Audio/video processing and conversion |
| pydub | Audio manipulation and chunking |
| PDF/TXT export | Downloadable meeting reports |

## ⚙️ Methods

1. Accept a YouTube URL or local audio/video file.
2. Extract and normalize audio.
3. Split long recordings into manageable audio chunks.
4. Transcribe speech using the selected language pipeline.
5. Generate a meeting title and summary.
6. Extract action items, decisions, and open questions.
7. Create transcript embeddings and store them in ChromaDB.
8. Retrieve relevant transcript passages for user questions.
9. Display results and allow PDF or TXT report export.

## 🎧 Audio Processing

Audio from supported online videos is downloaded using `yt-dlp` and processed with FFmpeg.

Local files are converted using pydub and FFmpeg into mono, 16 kHz WAV audio. Long recordings are divided into sequential 10-minute chunks to support manageable transcription and summarization.

## 🗣️ Speech Transcription

### English Transcription

- Uses locally executed OpenAI Whisper.
- The default model is `small`.
- Supports transcription without requiring a paid Whisper API.

### Hindi/Hinglish Transcription

- Uses Sarvam AI's speech-to-text translation endpoint.
- Audio chunks are further divided into 25-second subchunks to meet the endpoint's request-duration limit.

## 📝 Meeting Summarization

Mistral AI generates a concise meeting title and summary.

To handle longer transcripts, the system divides text into chunks of up to 3,000 characters, with 200 characters of overlap. It summarizes the chunks and combines the partial summaries into a consolidated result.

## 📋 Information Extraction

The system extracts useful meeting information, including:

- **Action Items:** Tasks, assigned owners, and deadlines when available.
- **Decisions:** Important decisions made during the meeting.
- **Open Questions:** Unresolved topics and follow-up questions.
- **Meeting Summary:** A concise overview of the discussion.

These outputs are AI-generated and should be reviewed for accuracy.

## 🔎 Retrieval-Augmented Generation (RAG)

The RAG pipeline allows users to ask questions about a meeting transcript.

### RAG Workflow

1. Split the transcript into chunks of approximately 500 characters with 50 characters of overlap.
2. Generate embeddings using Hugging Face's `all-MiniLM-L6-v2` model.
3. Store the embeddings in a persistent ChromaDB collection named `meeting_transcript`.
4. Convert the user's question into an embedding.
5. Retrieve the four most relevant transcript chunks.
6. Pass the question and retrieved context to Mistral AI.
7. Generate an answer grounded in the retrieved passages.

The prompt instructs the model to answer using the provided context and indicate when the answer is unavailable in that context.

## 🖥️ User Interface

The Streamlit interface in `app.py` provides:

- Source and language selection
- Processing status
- Meeting summary and insight panels
- Transcript-based question-answering interface
- Chat history display
- PDF and TXT report downloads

The command-line pipeline can also be run through `main.py`.

## ✨ Key Features

- YouTube URL and local audio/video input
- English and Hindi/Hinglish transcription pipelines
- Audio normalization and chunk-based processing
- AI-generated meeting titles and summaries
- Action-item, decision, and follow-up extraction
- RAG-powered questions and answers based on transcript content
- Persistent local vector storage with ChromaDB
- PDF and TXT report export
- Interactive Streamlit interface and command-line entry point

## 📊 Dashboard / Model / Output

The application provides the following outputs:

- Meeting title and summary
- Full speech transcript
- Action items with owners and deadlines when identified
- Key decisions and open questions
- Interactive question answering over retrieved transcript passages
- Downloadable PDF and TXT reports

## 🚀 How to Run

### 1. Clone the Repository

```bash
git clone <repository-link>
cd <project-folder>
```

Replace the placeholders with your actual repository URL and project directory.

### 2. Create and Activate a Virtual Environment

**Windows**

```bash
python -m venv venv
venv\Scripts\activate
```

**macOS / Linux**

```bash
python3 -m venv venv
source venv/bin/activate
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

If `requirements.txt` is not available, create it using the dependencies required by the project, such as Streamlit, Whisper, LangChain, ChromaDB, sentence-transformers, yt-dlp, pydub, and the relevant AI provider SDKs.

Install FFmpeg separately and ensure it is available on your system PATH.

### 4. Configure API Keys

Configure the Sarvam AI and Mistral AI API keys using the environment-variable names expected by your source code.

For example, if your code uses these names:

```env
SARVAM_API_KEY=your_sarvam_api_key
MISTRAL_API_KEY=your_mistral_api_key
```

Keep your API keys private and do not commit them to version control.

### 5. Run the Streamlit Application

```bash
streamlit run app.py
```

### 6. Run the Command-Line Pipeline

```bash
python main.py
```

## ⚠️ Limitations

- The application analyzes audio content, not visual information from video frames.
- Processing is sequential, so long recordings may take considerable time.
- Transcripts are plain text and do not currently provide speaker labels or timestamps.
- AI-generated summaries and extracted information may contain errors.
- RAG retrieval may include passages from an earlier meeting if the persistent ChromaDB collection is reused without meeting-specific isolation.
- Intermediate audio files may remain on disk after processing.
- Chat history is not incorporated into the RAG chain, so each answer primarily uses the current question and retrieved context.

## 🔮 Future Work

- Add speaker diarization and timestamped transcripts.
- Improve asynchronous processing and progress tracking for long recordings.
- Separate vector-store collections by meeting or recording ID.
- Validate extracted action items using structured output schemas.
- Improve temporary-file cleanup and secure handling of uploaded files.
- Strengthen HTML escaping for transcript and AI-generated content.
- Support conversational memory for follow-up questions within a meeting.
- Add automated tests and improve error handling for model and API failures.

## 👩‍💻 Author & Contact

**Pragati Tomar**

- **Email:** [officialpragatitomar@gmail.com](mailto:officialpragatitomar@gmail.com)
- **LinkedIn:** [linkedin.com/in/pragatitomar10](https://www.linkedin.com/in/pragatitomar10)
- **GitHub:** [github.com/Pragati-Tomar](https://github.com/Pragati-Tomar)

---

**AI Video Assistant** demonstrates how speech recognition, large language models, vector databases, and Retrieval-Augmented Generation can work together to make meeting recordings easier to understand, search, and review.
