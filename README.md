  # Noise-Robust Speech Recognition using Wav2Vec2
  
  Automatic Speech Recognition (ASR) systems trained on clean audio perform poorly in real-world noisy environments. This project improves speech recognition robustness by training a Wav2Vec2 model on clean and noise-augmented audio and evaluating performance under varying noise levels.

## Project Objectives

- Analyze the LibriSpeech dataset and understand speech characteristics.
- Implement controlled noise augmentation using the MUSAN noise dataset.
- Train two ASR models:
  - Clean-only trained model
  - Noise-augmented trained model
- Perform cross-evaluation under clean and noisy conditions.
- Compare performance using Word Error Rate (WER).

---

##  Dataset Details

### Clean Speech Dataset
- **Source:** LibriSpeech (train-clean-100)
- **Total Samples:** 28,539
- **Total Duration:** 100.59 hours
- **Unique Speakers:** 251
- **Sampling Rate:** 16 kHz

### Noise Dataset
- **Source:** MUSAN Noise Corpus
- **Total Noise Files:** 930
- Noise added dynamically with random SNR between **0–20 dB**

---

## ⚙️ Methodology

###  Data Preparation
- Extracted audio-transcript pairs from LibriSpeech.
- Normalized transcripts (lowercasing, character filtering).
- Verified dataset integrity (no missing or corrupted files).
- Analyzed duration, speech rate, silence ratio, and speaker distribution.

### Noise Augmentation
Noise is added using Signal-to-Noise Ratio (SNR):

\[
SNR(dB) = 10 \log_{10}\left(\frac{P_{clean}}{P_{noise}}\right)
\]

Noise is scaled dynamically to match a randomly sampled SNR level between 0–20 dB.

### Model Architecture
- Base model: `facebook/wav2vec2-base-960h`
- Framework: HuggingFace Transformers
- Model Type: `Wav2Vec2ForCTC`
- Training performed using PyTorch and HuggingFace Trainer API

Two models were trained:
- **Clean Model** → trained only on clean speech
- **Noisy Model** → trained with on-the-fly noise augmentation

---

## Evaluation Strategy

Models were evaluated under four conditions:

| Model        | Clean Evaluation | Noisy Evaluation |
|--------------|-----------------|-----------------|
| Clean Model  | 0.0109 WER      | 0.1001 WER      |
| Noisy Model  | 0.0153 WER      | 0.0371 WER      |

### Metric Used
- **Word Error Rate (WER)**  
- Character Error Rate (CER)

WER measures substitutions, insertions, and deletions relative to the ground truth transcript.

---

## Key Findings

- The clean-trained model performs best on clean data (≈1.1% WER).
- The clean-trained model degrades significantly under noise (≈10% WER).
- The noise-trained model maintains strong performance under both clean and noisy conditions.
- Noise training reduces WER under noisy conditions by approximately **63%** compared to the clean-only model.

This demonstrates that controlled noise augmentation significantly improves robustness with minimal impact on clean performance.

---

## Technologies Used

- Python
- PyTorch
- HuggingFace Transformers
- Librosa
- SoundFile
- Datasets
- Evaluate (WER, CER)
- Matplotlib / Seaborn

---

## Conclusion

This project demonstrates that noise-aware training substantially improves ASR robustness in noisy environments while maintaining competitive performance on clean speech.  

Noise augmentation is essential for deploying speech recognition systems in real-world conditions where background noise is unavoidable.

