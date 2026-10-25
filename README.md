# 🎥 YouTube Video Summarizer Using LangChain & OpenAI

An AI-powered application that extracts YouTube video transcripts and generates concise summaries using **LangChain, OpenAI, and Large Language Models (LLMs)**.

## 📌 Project Overview

Have you ever wanted to understand a 30-minute YouTube video without watching the entire video? 🤔

This project simplifies the process by extracting the video's transcript, splitting long transcripts into manageable chunks, and using OpenAI's language models to generate meaningful summaries.

The application demonstrates three approaches to summarization:

- **Stuff Chain:** Summarizes a video transcript directly when it fits within the model's context window.
- **Map-Reduce Chain:** Summarizes chunks of a long transcript and combines the intermediate summaries into a final summary.
- **Multiple Video Summarization:** Processes transcripts from multiple YouTube videos and combines their content to generate a consolidated summary.

## 🎯 Project Objectives

1. Extract transcripts from YouTube videos.
2. Retrieve video metadata, including the author's name and duration.
3. Generate concise summaries using OpenAI LLMs.
4. Split long transcripts into smaller chunks using `RecursiveCharacterTextSplitter`.
5. Implement different LangChain summarization strategies.
6. Summarize multiple YouTube videos in a single workflow.

## ✨ Key Features

- 🎬 YouTube transcript extraction using `YoutubeLoader`.
- 📝 Video metadata retrieval, including author and duration.
- 🤖 AI-powered summarization using OpenAI.
- ⚡ Fast summarization of shorter videos using the Stuff chain.
- 📚 Long-video summarization using the Map-Reduce chain.
- 🔗 Multi-video transcript processing and combined summarization.
- ✂️ Configurable text chunking with overlap.
- 🐍 Python implementation using LangChain.

## 🛠️ Tech Stack

| Technology | Purpose |
|---|---|
| Python | Core programming language |
| LangChain | LLM orchestration and summarization chains |
| OpenAI | Language model for generating summaries |
| YoutubeLoader | Extract YouTube transcripts and metadata |
| RecursiveCharacterTextSplitter | Split long transcripts into smaller chunks |
| python-dotenv (optional) | Manage API credentials through environment variables |

## 🔄 Project Workflow

```text
       YouTube Video URL
               |
               v
       YoutubeLoader
               |
               v
  Extract Transcript + Metadata
               |
               v
      Transcript Processing
               |
               v
     Text Chunking (if needed)
               |
               v
      LangChain Summarization
               |
       +-------+--------+
       |                |
       v                v
    Stuff Chain     Map-Reduce Chain
       |                |
       v                v
 Short Video       Long Video / Multiple
   Summary          Transcript Chunks
       |                |
       +-------+--------+
               |
               v
       Final AI Summary
```

## 🧠 Understanding the Summarization Strategies

### 1. Stuff Chain

```python
chain = load_summarize_chain(
    llm=llm,
    chain_type="stuff",
    verbose=True
)

print(chain.run(result))
```

The Stuff chain passes the supplied document content to the LLM in a single summarization operation.

**Best for:**
- Short transcripts.
- Documents that fit within the model's context window.
- Simple summarization workflows.

**Limitation:** Very long transcripts may exceed the model's context limit.

### 2. Map-Reduce Chain

```python
text_split = RecursiveCharacterTextSplitter(
    chunk_size=1000,
    chunk_overlap=200
)

texts = text_split.split_documents(result1)

chain = load_summarize_chain(
    llm=llm,
    chain_type="map_reduce",
    verbose=False
)

print(chain.run(texts))
```

The Map-Reduce approach works in two stages:

1. **Map:** Each transcript chunk is summarized separately.
2. **Reduce:** The intermediate summaries are combined into a final summary.

**Best for:**
- Long YouTube videos.
- Large transcripts that exceed a model's context window.
- Documents that need to be processed in smaller sections.

### 3. Multiple YouTube Video Summarization

```python
youtube_urls = [
    "https://youtu.be/h_EbSqu8KI4",
    "https://youtu.be/X2mbi6WLPnk"
]

text_split = RecursiveCharacterTextSplitter(
    chunk_size=1000,
    chunk_overlap=200
)

text = []

for url in youtube_urls:
    loader = YoutubeLoader.from_youtube_url(
        url,
        add_video_info=True
    )

    result = loader.load()

    print(
        f"Author: {result[0].metadata['author']}, "
        f"Duration: {result[0].metadata['length']} seconds"
    )

    text.extend(text_split.split_documents(result))

print(chain.run(text))
```

This workflow processes multiple YouTube URLs, extracts their transcripts, splits them into chunks, and combines the chunks for summarization.

The resulting summary can provide an overview of the content across the selected videos.

## 📂 Project Structure

```text
YouTube_Script_Summarize_Video/
│
├── YouTube_Script_Summarize_Video.ipynb  # Main notebook (if applicable)
├── constant.py                           # API key configuration (if used)
├── requirements.txt                      # Project dependencies
├── .gitignore                            # Files excluded from Git
└── README.md                             # Project documentation
```

*Note: Adjust the file names and structure to match your actual GitHub repository.*

## ⚙️ Installation and Setup

### 1. Clone the Repository

```bash
git clone https://github.com/KrishnakantAwasthi/YouTube_Script_Summarize_Video.git

cd YouTube_Script_Summarize_Video
```

### 2. Create a Virtual Environment

```bash
python -m venv venv
```

Activate it:

**Windows**
```bash
venv\Scripts\activate
```

**macOS / Linux**
```bash
source venv/bin/activate
```

### 3. Install Dependencies

Install the required libraries:

```bash
pip install langchain langchain-community langchain-openai youtube-transcript-api
```

Depending on your LangChain version and implementation, you may need to adjust imports and install additional dependencies.

### 4. Configure the OpenAI API Key

Set your API key as an environment variable.

**macOS / Linux**
```bash
export OPENAI_API_KEY="your_openai_api_key"
```

**Windows PowerShell**
```powershell
$env:OPENAI_API_KEY="your_openai_api_key"
```

Alternatively, use a `.env` file with `python-dotenv`:

```env
OPENAI_API_KEY=your_openai_api_key
```

Load it in Python:

```python
from dotenv import load_dotenv

load_dotenv()
```

**Security:** Never commit API keys, passwords, or other secrets to GitHub.

### 5. Run the Project

If your implementation is a Jupyter Notebook, launch it using:

```bash
jupyter notebook
```

Open the project notebook and run the cells in order.

If you have a Python script, execute it with:

```bash
python app.py
```

Replace `app.py` with your actual script filename.

## 📖 Important Code Concepts

| Component | Explanation |
|---|---|
| `YoutubeLoader.from_youtube_url()` | Loads a video's transcript and optionally its metadata. |
| `loader.load()` | Returns the loaded content as a list of LangChain documents. |
| `result[0].page_content` | Accesses the transcript text of the first document. |
| `result[0].metadata` | Contains available metadata, such as author and video duration. |
| `OpenAI(temperature=0.8)` | Configures a language model with a sampling temperature. |
| `RecursiveCharacterTextSplitter` | Splits text into chunks while attempting to preserve natural text boundaries. |
| `chunk_size=1000` | Sets the target chunk size; the unit is characters by default. |
| `chunk_overlap=200` | Retains overlap between adjacent chunks to help preserve context. |
| `load_summarize_chain()` | Creates a LangChain summarization chain. |
| `chain.run()` | Runs the summarization chain in older LangChain interfaces. |

## ⚠️ Notes and Limitations

- YouTube transcript extraction depends on transcript availability and access permissions.
- Some videos may not have captions or transcripts available.
- The Stuff chain may fail when the transcript exceeds the model's context window.
- Map-Reduce summarization can require more LLM calls and incur higher API costs.
- The quality of the final summary depends on transcript accuracy and the language model.
- The OpenAI API requires a valid API key and may incur usage charges.

**LangChain compatibility:** The code uses older interfaces such as `langchain.llms.OpenAI`, `load_summarize_chain`, and `chain.run()`. In newer LangChain versions, use the appropriate packages and interfaces, such as `langchain_openai.ChatOpenAI`, and verify compatibility with the installed version.

## 🚀 Future Improvements

- Build a web interface using Streamlit.
- Display video titles, URLs, authors, and durations alongside summaries.
- Allow users to summarize playlists and multiple videos.
- Add customizable summary length and output formats.
- Generate bullet-point summaries and key takeaways.
- Extract timestamps for important topics.
- Support multilingual transcripts and summaries.
- Add caching to reduce repeated API calls and costs.
- Explore Retrieval-Augmented Generation (RAG) for question-answering over video transcripts.

## 🔗 GitHub Repository

Explore the complete project and source code:

**[YouTube_Script_Summarize_Video — GitHub Repository](https://github.com/KrishnakantAwasthi/YouTube_Script_Summarize_Video/tree/main)**

## 👨‍💻 Author

**Krishnakant Awasthi**

- GitHub: [Link](https://www.linkedin.com/in/krishnakantawasthi)
- LinkedIn: [Link](https://github.com/KrishnakantAwasthi)

## ⭐ Support

If you find this project useful, consider giving the repository a ⭐ on GitHub!

---

**Built with Python, LangChain, OpenAI, and YouTube transcript extraction.**
