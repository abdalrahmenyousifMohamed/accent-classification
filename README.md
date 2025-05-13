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

