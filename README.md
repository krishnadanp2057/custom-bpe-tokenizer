# BPE Subword Tokenizer on Wikipedia AI Articles

A small end-to-end NLP project: scrape text from Wikipedia, clean it, and train a Byte Pair Encoding (BPE) subword tokenizer using Hugging Face `tokenizers`.

## What it does

1. **Scrapes** paragraph text from four Wikipedia articles:
   - Artificial intelligence
   - Machine learning
   - Deep learning
   - Generative artificial intelligence
2. **Cleans** the text by removing citation markers such as `[ 1 ]`, collapsing extra whitespace, and dropping very short lines.
3. **Trains** a BPE tokenizer on the cleaned corpus.
4. **Saves** the trained tokenizer to `ai_subword_tokenizer.json` and tokenizes a sample sentence.

## Tokenizer settings

| Setting | Value |
|---|---|
| Algorithm | BPE |
| Vocabulary size | 5000 |
| Minimum frequency | 2 |
| Pre-tokenizer | Whitespace |
| Special tokens | `<UNK>`, `<PAD>`, `<BOS>`, `<EOS>` |

## Project structure

```
.
├── README.md
├── requirements.txt
├── scrape_corpus.py          # scrape + clean Wikipedia text
├── train_tokenizer.py        # train and save the BPE tokenizer
└── ai_subword_tokenizer.json # trained tokenizer
```

## Setup

```bash
pip install -r requirements.txt
```

`requirements.txt`:
```
requests
beautifulsoup4
tokenizers
```

## Usage

**1. Build the corpus**
```bash
python scrape_corpus.py
```
This writes `ai_corpus.txt` (raw) and `cleaned_ai_corpus.txt` (cleaned).

**2. Train the tokenizer**
```bash
python train_tokenizer.py
```
Make sure the training script reads `cleaned_ai_corpus.txt`, not the raw file.

**3. Load and use the tokenizer**
```python
from tokenizers import Tokenizer

tokenizer = Tokenizer.from_file("ai_subword_tokenizer.json")

sentence = "Agentic AI systems use generative models for autonomous decision making"
encoded = tokenizer.encode(sentence)

print(encoded.tokens)
print(encoded.ids)
```

## Example output

Run the snippet above and paste your own output here:

```
Tokens: [...]
```

## Limitations

- The corpus is small (four articles), so the vocabulary is limited and some tokens are noisy.
- Wikipedia's math markup (for example `{\displaystyle ...}`) can leak into the text unless it is stripped during cleaning.
- No quantitative evaluation yet.

## Possible improvements

- Implement BPE from scratch in plain Python and compare it with the library version.
- Measure compression (tokens per word) at different vocabulary sizes and against a standard tokenizer such as GPT-2's.
- Expand the corpus and strip LaTeX leftovers during cleaning.
- Use the tokenizer to train a small language model or text classifier.

## Data source and license

Text is scraped from Wikipedia and is licensed under [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/). Source pages:

- https://en.wikipedia.org/wiki/Artificial_intelligence
- https://en.wikipedia.org/wiki/Machine_learning
- https://en.wikipedia.org/wiki/Deep_learning
- https://en.wikipedia.org/wiki/Generative_artificial_intelligence

## Author

Krishnandan Pandit
