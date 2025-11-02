# CIFAR-10 Prompt-Driven Vision Evaluation

A command-line benchmark evaluating a vision-language model’s ability to classify **CIFAR-10** images (32×32) via the SoonerAI platform. The script samples 100 images (10 per class), sends them to the model, and evaluates accuracy across different textual prompts.

---

## Setup

### Python Version
Requires **Python 3.8+**

Install dependencies from the project root:

    pip install -r requirements.txt

---

## SoonerAI API Configuration

1. Visit https://ai.sooners.us
2. Register with your OU email and log in
3. Navigate to Settings → Account → API Keys
4. Create an API key

### Create `.soonerai.env` in your home directory

Linux / macOS:

    nano ~/.soonerai.env

Windows PowerShell:

    notepad $env:USERPROFILE\.soonerai.env

Add the following:

    SOONERAI_API_KEY=your_key_here
    SOONERAI_BASE_URL=https://ai.sooners.us
    SOONERAI_MODEL=gemma3:4b

Replace 'your_key_here' with your API key then Save and close.

---

## Running the Evaluation

Run from the project root:

    python cifar10_eval.py

This script will:

- Download CIFAR-10
- Sample 100 images (10 per class)
- Send images to the model
- Compute accuracy
- Produce a confusion matrix and misclassification log

Artifacts produced:

    confusion_matrix.png
    misclassifications.jsonl

---

## Results Summary and Prompt Analysis

This benchmark demonstrates how prompt engineering influences CIFAR-10 classification accuracy in a multimodal LLM.

### Experimental Constants
- Model: gemma3:4b (SoonerAI backend)
- Dataset: CIFAR-10 (train set)
- Images evaluated: 100 (10 per class)
- Temperature: 0 (deterministic)
- Same seed for all runs

### Accuracy by Prompt Version

| Prompt Version | Accuracy | Description |
|----------------|----------|-------------|
| Prompt 1 | 56% | Basic label instruction |
| Prompt 2 | 56% | Adds CIFAR-10 context and low-resolution note |
| Prompt 3 | 59% | Adds explicit class-pair distinctions |
| Prompt 4 | 63% | Morphology-driven guidance, especially for frogs |

### Interpretation

Performance improved from **56% → 63%** purely through prompt refinement.  
Observations:

- Mentioning **image resolution** and emphasizing **shape-based cues** improved accuracy
- Highlighting **confusion pairs** (cat-dog, deer-horse, bird-frog) reduced systematic errors
- Frog classification improved significantly when color and posture criteria were stated
- This illustrates the value of structured prompt engineering for multimodal reasoning under low-resolution constraints, even without model retraining or architectural changes

---

## Confusion Matrices

Stored in:

    matrices/confusion_matrix1.png
    matrices/confusion_matrix2.png
    matrices/confusion_matrix3.png
    matrices/confusion_matrix4.png

Each corresponds to Prompts 1–4.

---

## Stopping Execution

To exit:

- Ctrl + C, or
- close the terminal

---

## Future Extensions

- Prompt ensembling and majority voting
- Few-shot visual prompting examples
- Test-time augmentation (flip / crop / upscale)
- CNN baseline comparison (e.g., ResNet-18 on CIFAR-10)

---

## Notes

- CIFAR-10 (32×32) presents a challenge for general-purpose VLMs
- This project isolates prompt quality as the primary variable
- Results demonstrate prompt-driven performance gains in low-resolution recognition tasks

---

End of README
