# 🗣️ Accent Classification Tool

## 📌 Features

- Accepts any public video URL (YouTube, Loom, direct MP4 links, etc.)
- Extracts and processes spoken English audio
- Performs accent classification using a pre-trained model
- Outputs:
  - Detected accent (e.g., us, england, india)
  - Confidence score (0–100%)
  - Optional explanation for internal evaluation

---

## 🧠 How It Works

### Step-by-Step Process

#### 1. Download & Extract Audio

Supports both streaming links and direct MP4s using:
- `yt-dlp` for YouTube and streaming platforms
- `requests` + `ffmpeg` for direct downloads

#### 2. Transcribe Speech

Uses OpenAI's Whisper ASR to transcribe spoken English accurately.

#### 3. Classify Accent

Uses a pre-trained ECAPA-TDNN model from SpeechBrain:
- Model: `Jzuluaga/accent-id-commonaccent_ecapa`
- Classifies into 16 English accents (e.g., us, england, australia, india)

#### 4. Display Results

- Predicted Accent Label
- Confidence Score (0–100%)
- Optional explanation (e.g., for hiring context)



## 🧪 Example Output

```
Enter video URL: https://www.youtube.com/watch?v=dQw4w9WgXcQ
Downloading and extracting audio...
Transcribing audio...
Transcription: Hello everyone, my name is Rick and I'm here to talk about...
Classifying accent...
Accent: us (91.2%)
Explanation: Strong acoustic cues match typical American English speech patterns.
```

---

## 🛠️ Built With

- `yt-dlp` – video downloader
- `OpenAI Whisper` – ASR transcription
- `SpeechBrain` – ECAPA-TDNN accent classifier
- `FFmpeg` – audio extraction and conversion
- `Python 3.8+` – core language

---

## 📥 Requirements

Install with:

```bash
pip install yt-dlp openai-whisper speechbrain librosa numpy requests
```

Ensure `ffmpeg` is installed and accessible via command line.

---

## 🙋 FAQ

**Q: Can it work offline?**  
A: Yes. Once models and dependencies are downloaded, Whisper and SpeechBrain can run offline.

**Q: What accents can it detect?**  
A: Supports 16 English accents: us, england, ireland, india, australia, canada, philippines, etc.

**Q: What if the video has music or noise?**  
A: Whisper is robust but accuracy improves with clear speech and minimal noise.

- The provided system can recognize the following 16 accents from short speech recordings in English (EN):
```
african
australia
bermuda
canada
england
hongkong
indian
ireland
malaysia
newzealand
philippines
scotland
singapore
southatlandtic
us
wales
```