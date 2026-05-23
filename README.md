# PsychLing Benchmark: Supplementary Metric Pipeline

This repository contains the supplementary notebook for the ACL submission
*PsychLing Benchmark: Psycholinguistic metrics for Human-Model Corpus Comparison in Children and Adults*.

The notebook provides a compact, reproducible implementation of the benchmark metrics that can be computed for a pair of German texts. It is intended as anonymous review material: it contains the executable analysis pipeline and example input texts, but does not include author-identifying information.

## Repository Contents
add
- `PsychLing_Benchmark.ipynb`: Jupyter notebook implementing the
  supplementary metric pipeline.
- `README.md`: this file.

No additional data files are required. The two example texts are defined directly in the notebook in the `TEXTS` dictionary and can be replaced with other German texts.

## What the Notebook Computes

The notebook covers four psycholinguistic levels:

- **Textual and lexical measures**: token counts, type counts, mean tokens per text, mean types per text, median word length, median log word frequency, MTLD, and HD-D.
- **Part-of-speech measures**: counts and percentages for the six POS categories: `ADJ`, `ADV`, `DET`, `VERB`, `NOUN`, and `PRON`.
- **Syntactic measures**: sentence length, tree depth, closing nodes, open nodes, and absolute dependency distance, using Stanford CoreNLP German parses through `stanza`.
- **Semantic similarity measures**: lexical-level semantic similarity via representational similarity analysis (RSA) over contextual German BERT embeddings, and text-level semantic similarity via cosine similarity over multilingual sentence embeddings.

## Runtime Requirements

The notebook is designed to run in Google Colab or a fresh local Python environment. It installs missing Python packages from within the notebook.

Main dependencies:

- Python 3.9 or newer
- `pandas`
- `numpy`
- `scipy`
- `spacy`
- `lexical-diversity`
- `torch`
- `transformers`
- `sentence-transformers`
- `sentencepiece`
- `stanza`
- `nltk`
- German spaCy model: `de_core_news_sm`
- Stanford CoreNLP with German models for the syntactic section

The notebook setup cells download the German spaCy model, Stanford CoreNLP, and the required transformer models when they are not already available. The semantic section uses:

- `google-bert/bert-base-german-cased`
- `sentence-transformers/paraphrase-multilingual-MiniLM-L12-v2`

## How to Run

1. Open `PsychLing_Benchmark.ipynb` in Jupyter, JupyterLab, or Google Colab.
2. Run the notebook from top to bottom.
3. To analyze different texts, edit only the string values in the `TEXTS`
   dictionary near the top of the notebook:

   ```python
   TEXTS = {
       "Text 1": "...",
       "Text 2": "...",
   }
   ```

4. Run all cells again.

## CoreNLP Configuration

The syntax section uses Stanford CoreNLP through `stanza.server.CoreNLPClient`. By default, the notebook looks for an existing installation via `CORENLP_HOME`. If `CORENLP_HOME` is not set, it attempts to install CoreNLP into a local `corenlp/` directory next to the notebook.

Relevant settings in the notebook:

```python
CORENLP_HOME = os.environ.get("CORENLP_HOME") or str(Path.cwd() / "corenlp")
CORENLP_VERSION = os.environ.get("CORENLP_VERSION", "4.5.10")
INSTALL_CORENLP_IF_MISSING = True
CORENLP_PORT = int(os.environ.get("CORENLP_PORT") or find_open_port())
CORENLP_ENDPOINT = f"http://localhost:{CORENLP_PORT}"
CORENLP_PROPERTIES = "german"
```

If CoreNLP or the German model jar is unavailable, the notebook reports that syntax metrics are unavailable and continues with the other metric sections.
Set `CORENLP_PORT` before running the notebook only if a fixed port is required; otherwise, the notebook chooses an available local port automatically.

## Outputs

The notebook displays the following tables:

- `summary`: aggregate lexical and textual measures.
- `pos_summary`: aggregate POS counts and percentages.
- `per_text`: per-text lexical, textual, and POS measures.
- `syntax_summary`: aggregate syntactic measures.
- `syntax_per_text`: per-text syntactic measures.
- `syntax_per_token`: token-level syntactic parse and dependency information.
- `semantic_summary`: lexical- and text-level semantic similarity results.
- `semantic_shared_words`: shared word types used for lexical semantic RSA.


<!-- ## Reproducibility Notes

- Semantic sampling is seeded with `SEMANTIC_SEED = 42`.
- GPU is used automatically for transformer models when `torch.cuda.is_available()`
  returns `True`; otherwise, the notebook runs on CPU.
- First runs may take longer because spaCy, CoreNLP, and transformer models may
  need to be downloaded.
- Small numerical differences can occur across hardware, library versions, and
  CPU/GPU execution.

## Scope of the Supplementary Artifact

This artifact is meant to support inspection and rerunning of the metric
implementation. It is not a full release of the underlying corpora used in the
paper and does not include the paper's downstream statistical analyses. -->
