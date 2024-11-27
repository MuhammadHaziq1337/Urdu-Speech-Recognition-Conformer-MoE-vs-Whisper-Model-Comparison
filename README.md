# Urdu Speech Recognition: Conformer-MoE vs Whisper Model Comparison

## Overview

This project involves the implementation and performance comparison of two Urdu speech recognition models:
1. **Conformer-MoE (Mixture of Experts)**: A custom-built architecture.
2. **Whisper Model**: A pre-trained model from OpenAI.

### Dataset
The experiments were conducted using the Mozilla Common Voice Urdu dataset (Version 11.0), containing:

| **Dataset Split** | **Number of Samples** |
|--------------------|------------------------|
| Training           | 4,129                 |
| Validation         | 3,303                 |
| Test               | 3,302                 |

---

## Conformer-MoE Architecture

### Model Components
- **Input Processing**:
  - Mel spectrogram features (80 dimensions).
  - Audio resampling to 16kHz.
  - Batch processing with gradient accumulation.
- **Core Architecture**:
  - Input projection layer, positional encoding.
  - 4 conformer blocks, each consisting of:
    - Multi-head self-attention.
    - Convolution module.
    - Feed-forward module.
    - Mixture of Experts (MoE) layer.

### Training Configuration
| **Parameter**               | **Value**            |
|-----------------------------|----------------------|
| Batch size                  | 8                    |
| Gradient accumulation       | 2 steps             |
| Learning rate               | 0.0001              |
| Optimizer                   | Adam                |
| Loss function               | CTC Loss            |
| Mixed precision training    | FP16                |
| Early stopping              | Patience of 5       |
| Scheduler                   | ReduceLROnPlateau   |

### Training Process
| **Metric**       | **Initial Value** (Epoch 0) | **Final Value** (Epoch 10) | **Best Value** (Epoch 9) |
|-------------------|----------------------------|----------------------------|--------------------------|
| Validation Loss   | 1.6217                    | 1.3224                    | 1.3123                  |
| Validation WER    | 1.0116                    | 1.0588                    | -                       |

---

## Whisper Model Integration

- **Model Used**: Whisper-large-v2.
- **Custom Adaptations**:
  - Custom audio preprocessing.
  - 16kHz resampling for compatibility.
  - Batch processing optimized for memory efficiency.
- **Performance**: Achieved WER: **0.3241**.

---

## Performance Comparison

| **Metric**       | **Whisper**  | **Conformer-MoE** |
|-------------------|--------------|-------------------|
| Mean WER          | 0.3241       | 1.1624            |
| Median WER        | 0.2500       | 1.0000            |
| Min WER           | 0.0909       | 0.9444            |
| Max WER           | 1.0000       | 1.6667            |

---

## Sample Predictions

| **Example #** | **Reference**                                | **Whisper Prediction**                        | **Conformer Prediction**                    |
|---------------|----------------------------------------------|-----------------------------------------------|---------------------------------------------|
| 1             | بھی کا 'یوتھ' تناسب یہی ہے۔                  | ۔ ے ہ ے ہ ی ہ ک اہ ہے بھی کا یوت تناسب یہی    | ۔ ے ہ ے ہ ہ ک ک ک اہ ہوا ثابت وفا بے        |
| 2             | بھوک ہوا ثابت وفا بے                         | -                                             | ہوا ثابت وفا بے                             |

---

## Analysis

| **Aspect**            | **Whisper Strengths**                     | **Conformer-MoE Strengths**              |
|------------------------|-------------------------------------------|------------------------------------------|
| Accuracy               | Superior overall accuracy.                | Trainable end-to-end.                    |
| Consistency            | More consistent predictions.              | Potential for domain adaptation.         |
| Complex Sentences      | Handles complex structures.               | Lower latency in inference.              |

| **Aspect**            | **Whisper Challenges**                    | **Conformer-MoE Challenges**             |
|------------------------|-------------------------------------------|------------------------------------------|
| Prediction Issues      | Fixed architecture limits adaptability.   | Character-level errors.                  |
| Resource Usage         | High memory and latency.                  | Training instability.                    |
| Performance            | Longer inference time.                    | Higher WER.                              |

---

## Recommendations

1. **Future Improvements for Conformer-MoE**:
   - Experiment with more experts or advanced character-to-word mapping techniques.
2. **Optimizing Whisper**:
   - Investigate techniques to reduce inference time and explore Urdu-specific fine-tuning.
3. **Dataset Enhancements**:
   - Incorporate diverse and noisy audio samples and balance dialect representation.
4. **Additional Comparisons**:
   - Evaluate transformer-based models like Wav2Vec 2.0 or HuBERT.

---

## Conclusion

This comparison highlights the strengths and limitations of both models for Urdu speech recognition. While Whisper outperformed in terms of accuracy, the Conformer-MoE model offers flexibility and domain adaptability. Future work should focus on optimizing both architectures and expanding the dataset for enhanced performance.
