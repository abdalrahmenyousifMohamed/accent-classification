#  Whisper ASR + Pyannote for Accent Categorization

##  Introduction

Accent categorization is the task of identifying a speaker’s accent from spoken audio (e.g., Indian English, American English, British English). It plays a vital role in:

- Speech analytics  
- Language learning technology  
- Tuning speech-to-text models  
- Quality assurance for call centers  
- Regional content localization  

This project combines **speaker diarization** and **automatic speech recognition (ASR)** to segment, transcribe, and analyze speech per speaker to determine their accent.

---

##  Technologies Used

| Component               | Purpose                            | Tool                                |
|------------------------|------------------------------------|-------------------------------------|
| ASR                    | Transcribe spoken words            | OpenAI Whisper                      |
| Speaker Diarization    | Identify and segment speakers      | pyannote-audio                      |
| Timestamp Alignment    | Align segments with transcript     | Custom timestamp merging logic      |
| Feature Extraction     | Extract embeddings for analysis    | e.g., x-vectors, Wav2Vec2, MFCC     |
| Accent Classifier      | (Optional) Classify accents        | Custom model (XGBoost, Transformer) |

---

##  Why This Setup Works for Accent Categorization

### 1.  Speaker-Aware Accent Detection  
- **Challenge**: Accent is a speaker-level trait, not per utterance.  
- **Solution**: Use diarization to group all audio per speaker.  
- **Benefit**: More accurate and personalized accent categorization.

### 2.  Whisper’s Robust Multilingual Transcription  
- Trained on diverse, multi-accent corpora  
- Maintains performance across heavily-accented English  
- Transcripts can support phoneme-level or linguistic feature analysis

### 3.  Pyannote’s Effective Speaker Segmentation  
- Strong performance in real-world noisy scenarios  
- Supports overlapping speech  
- Provides speaker embeddings to group identity across segments

### 4.  ECAPA-TDNN for Speaker Embedding Extraction
- Utilizes the speechbrain/spkrec-ecapa-voxceleb model
- Extracts high-quality speaker embeddings using attentive statistical pooling
- Embeddings can be used for clustering and accent classification

> This pipeline becomes a **scalable preprocessing system** for accent analysis.

---

##  Workflow Summary

```plaintext
Input Audio
   ↓
[pyannote-audio]
→ Diarize: (speaker, start, end)
   ↓
[Whisper]
→ Transcribe + Timestamps
   ↓
Merge Transcripts with Speaker Segments
   ↓
Per Speaker:
    → Extract audio from all turns
    → (Optional) Normalize loudness, remove silences
    → Extract acoustic embeddings
    → Run accent classifier or clustering
   ↓
Output: (accent label)
```

---

##  Implementation Notes

- **Speaker Embedding Extraction**:  the speechbrain/spkrec-ecapa-voxceleb model to extract 192-dimensional speaker embeddings.
- **Clustering Speaker Embeddings**: Helps correct diarization errors and improves speaker grouping.
- **Language Filtering**: Whisper’s language ID can validate that the audio is English before accent classification.
- **Accent Training Data**:  labeled datasets like Common Voice (with accent tags) or L2-Arctic for training classifiers.

---

##  Limitations

- **Diarization Errors**: False splits/merges can skew accent predictions.  
- **ASR Bias**: Whisper may transcribe differently depending on accent.  
- **Training Data**: Accent classifiers need well-curated, labeled datasets.  
- **Granularity**: Accent is a spectrum—broad labels (e.g., "British", "American") may oversimplify real-world nuances.

---

# Seconed Option 
#  Strategy 2 : Accent Classification Tool

##  Features

- Accepts any public video URL (YouTube, Loom, direct MP4 links, etc.)
- Extracts and processes spoken English audio
- Performs accent classification using a pre-trained model
- Outputs:
  - Detected accent (e.g., us, england, india)
  - Confidence score (0–100%)
  - Optional explanation for internal evaluation

---

##  How It Works

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



##  Example Output

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

## 🛠 Built With

- `yt-dlp` – video downloader
- `OpenAI Whisper` – ASR transcription
- `SpeechBrain` – ECAPA-TDNN accent classifier
- `FFmpeg` – audio extraction and conversion
- `Python 3.8+` – core language

---

##  Requirements

Install with:

```bash
pip install yt-dlp openai-whisper speechbrain librosa numpy requests
```

Ensure `ffmpeg` is installed and accessible via command line.

---

##  FAQ

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
