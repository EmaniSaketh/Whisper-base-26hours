# Bilingual Telugu + English Whisper ASR

This project fine-tunes the OpenAI Whisper Base model for Automatic Speech Recognition (ASR) using Telugu and English speech data.

## Model

- Base model: Whisper Base
- Languages: Telugu + English
- Training epochs: 3
- Training steps: 3,915
- Training GPU: NVIDIA T4
- Final checkpoint: `checkpoint-3915`

## Results

| Dataset | Samples | WER | CER |
|---|---:|---:|---:|
| English Test | 713 | 6.07% | 2.03% |
| Telugu Validation | 903 | 45.25% | — |

---

# Running the Project in Google Colab

## Requirements

- Google account
- Google Colab
- Google Drive
- Internet connection
- GPU is recommended for inference/training

> The notebook was developed and tested in Google Colab.

---

# 1. Open the Colab Notebook

Open the shared Google Colab link provided with this project.

If Colab asks you to sign in, sign in using your Google account.

Then select:

`Runtime → Change runtime type → T4 GPU`

GPU availability depends on your Colab account and current usage limits.

> If a GPU is unavailable, the trained model can still be loaded on CPU for small inference tests, although inference will be slower.

---

# 2. Run the Notebook from the Beginning

Run the notebook cells in order.

Use:

`Runtime → Run all`

or execute each cell individually using the ▶ button.

---

# 3. Mount Google Drive

The notebook will ask for permission to access Google Drive.

Run:

```python
from google.colab import drive
drive.mount('/content/drive')

A Google authorization link may appear.

Open the link.
Select your Google account.
Allow Google Colab to access Drive.
Copy the authorization code if requested.
Paste it into Colab.

The project files should then be accessible under:

/content/drive/MyDrive/
4. Required Google Drive Structure

The trained model and evaluation files are stored in Google Drive.

The expected structure is:

MyDrive/
│
├── whisper_bilingual_checkpoints/
│   ├── checkpoint-3000/
│   ├── checkpoint-3500/
│   └── checkpoint-3915/
│
├── english_evaluation_results.csv
│
├── telugu_validation_1.5h/
│   ├── audio/
│   └── text/
│
└── english_test_1.5h/
    ├── audio/
    └── text/

The final trained model is:

/content/drive/MyDrive/whisper_bilingual_checkpoints/checkpoint-3915
5. Check the Checkpoints

Run:

import os

checkpoint_dir = "/content/drive/MyDrive/whisper_bilingual_checkpoints"

print("Available checkpoints:")

for item in sorted(os.listdir(checkpoint_dir)):
    path = os.path.join(checkpoint_dir, item)

    if os.path.isdir(path):
        print("✓", item)

Expected output:

✓ checkpoint-3000
✓ checkpoint-3500
✓ checkpoint-3915
6. Load the Fine-Tuned Whisper Model

Run:

import torch
from transformers import WhisperProcessor, WhisperForConditionalGeneration

checkpoint_path = "/content/drive/MyDrive/whisper_bilingual_checkpoints/checkpoint-3915"

processor = WhisperProcessor.from_pretrained(checkpoint_path)
model = WhisperForConditionalGeneration.from_pretrained(checkpoint_path)

device = "cuda" if torch.cuda.is_available() else "cpu"

model = model.to(device)
model.eval()

print("Model loaded successfully")
print("Device:", device)
print("Checkpoint:", checkpoint_path)

Expected output will show:

Model loaded successfully
Device: cuda
Checkpoint: /content/drive/MyDrive/whisper_bilingual_checkpoints/checkpoint-3915

If GPU is unavailable:

Device: cpu

This is still sufficient for testing individual audio files.

7. Test an English Audio File

Example audio:

1580-141083-0024.flac

Path:

/content/drive/MyDrive/english_test_1.5h/audio/1580-141083-0024.flac

The notebook can load the audio, pass it through the fine-tuned model and display the transcription.

The expected example is:

YOU LEFT HIM IN A CHAIR YOU SAY WHICH CHAIR BY THE WINDOW THERE
8. Test a Telugu Audio File

Example:

000010042.wav

Path:

/content/drive/MyDrive/telugu_validation_1.5h/audio/000010042.wav

The notebook can run the audio through the model using the Telugu decoding configuration.

The reference transcription for this example is:

చింటూ అలియాస్ చంద్రశేఖర్ చంద్రశేఖర్ నివాసంలో పోలీసులు సోదా చేసి

The model prediction was:

చింటూ అల్యాస్ చంద్రశేఖర్ నివాసను పోలీసులు సోదా చేసి
9. English Evaluation Results

The complete English evaluation has already been performed.

Results:

Test samples : 713
WER          : 6.07%
CER          : 2.03%

The detailed predictions are stored in:

/content/drive/MyDrive/english_evaluation_results.csv

The CSV contains:

audio_file
reference
prediction

To view it:

import pandas as pd

path = "/content/drive/MyDrive/english_evaluation_results.csv"

df = pd.read_csv(path)

print("Number of samples:", len(df))

display(df.head(10))
10. Telugu Evaluation Results

The Telugu validation set contains:

903 audio samples
903 reference transcripts

Evaluation result:

WER = 45.25%

The Telugu evaluation was performed using the final:

checkpoint-3915
11. Important: Training Does NOT Need to Be Repeated

The shared notebook contains the workflow and inference/evaluation steps.

The final trained checkpoint is already available in Google Drive:

checkpoint-3915

Therefore, if the purpose is only to:

load the trained model
test audio
demonstrate inference
inspect evaluation results

you do not need to train the model again.

Simply mount Drive and load:

checkpoint-3915
12. If You Want to Train Again

Training from scratch requires the training datasets and the required dependencies.

The training process used approximately:

Telugu      : 11.90 hours
English     : 15.00 hours
Total       : 26.90 hours

Training was completed for:

Epochs      : 3
Steps       : 3,915
GPU         : NVIDIA T4

Training from scratch will require substantially more time and GPU resources than simply loading the saved checkpoint.

13. Important Notes for a New User
Google Drive

The shared Colab notebook does not automatically give another user access to your private Google Drive files.

If another person wants to run the notebook using the saved model, they need access to the required project files in their own Drive or through whatever shared storage arrangement is provided with the project.

Colab GPU

GPU availability depends on the user's Colab account and current usage limits.

If a T4 GPU is unavailable, the notebook can still be used for small CPU-based inference tests.

File Paths

If the user stores the project folders in a different location, update the paths used in the notebook.

For example:

checkpoint_path = "/content/drive/MyDrive/whisper_bilingual_checkpoints/checkpoint-3915"

must point to the user's actual checkpoint location.

14. Project Workflow

The overall workflow is:

Telugu Dataset
       +
English Dataset
       ↓
Dataset Preparation
       ↓
Whisper Base
       ↓
Bilingual Fine-Tuning
       ↓
Training Checkpoints
       ↓
checkpoint-3915
       ↓
Evaluation
       ↓
WER / CER
       ↓
Real Audio Inference
15. Final Model

The final model used for demonstration is:

Whisper Base
Fine-tuned on Telugu + English
Checkpoint: checkpoint-3915

Final evaluated results:

English WER : 6.07%
English CER : 2.03%

Telugu WER  : 45.25%
16. Recommended Order for Demonstration

For a quick project demonstration, run the notebook in this order:

1. Mount Google Drive
        ↓
2. Check checkpoints
        ↓
3. Load checkpoint-3915
        ↓
4. Test English audio
        ↓
5. Test Telugu audio
        ↓
6. Open English evaluation CSV
        ↓
7. Display evaluation results

This allows the trained bilingual Whisper model to be demonstrated without repeating the complete training process.


**One important point:** because this is a **shared Colab**, don't put your personal Google Drive authorization, private credentials, or private dataset links directly into the README. The person opening the notebook should connect **their own Drive** and provide/access the required files.
