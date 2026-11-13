# DeepRNAi public dataset and model evaluation

This repository contains the DeepRNAi public dataset, comprising 240 siRNAs across the training and test splits, and a Jupyter notebook for comparing model performance on the public test set.

## Repository contents

- **Evaluation notebook:** plots and compares predictions from mini-DeepRNAi, mini-DeepRNAi without biological foundation model (BioFM) embeddings, and mini-Random Forest.
- **`deeprnai_dataset.csv.gz`:** compressed CSV containing the public training and test data, experimental measurements, and model predictions.

## Dataset

Each row represents an siRNA–transcript pair, linking an siRNA to the measured expression change of a readout transcript. Each siRNA therefore appears in multiple rows.

The `target_gene_name` column identifies the intended target of the siRNA, whereas `readout_gene_name` identifies the gene whose expression was measured. The `split` column specifies the predefined training or test assignment.

### Data columns

| Column | Description |
| --- | --- |
| `sirna_id` | Identifier for the siRNA. |
| `sirna_guide_seq` | Guide-strand nucleotide sequence. |
| `sirna_synthesis` | Synthesis identifier or annotation for the siRNA. |
| `readout_transcript_id` | Identifier of the transcript whose expression was measured. |
| `readout_gene_name` | Gene associated with the readout transcript. |
| `target_gene_name` | Intended target gene of the siRNA. |
| `cell_line` | Cell line used in the experiment. |
| `chemistry` | Label identifying the siRNA chemistry. |
| `format` | Label identifying the siRNA format. |
| `split` | Dataset assignment: `train` or `test`. |
| `baseline_tpm` | Baseline transcript expression in transcripts per million (TPM). |
| `log2fc_centered` | Centered log₂ fold change in expression following siRNA treatment. Negative values indicate reduced expression. |
| `padj_diffexp` | Adjusted p-value from differential expression analysis. |
| `best_match_type` | Best assigned sequence-match category for the siRNA–transcript pair. |
| `mini_deeprnai_preds` | Predicted expression change from mini-DeepRNAi. |
| `mini_deeprnai_no_biofms_preds` | Predicted expression change from mini-DeepRNAi without BioFM embeddings. |
| `mini_random_forest_preds` | Predicted expression change from mini-Random Forest. |

Missing values, including unavailable model predictions, are represented as `NaN`.

### Loading the data

Pandas can read the compressed CSV directly:

```python
import pandas as pd

df = pd.read_csv("deeprnai_dataset.csv.gz")

train_df = df.loc[df["split"] == "train"].copy()
test_df = df.loc[df["split"] == "test"].copy()
```

### License.
This dataset is made available under the Creative Commons Attribution 4.0 International Public License.
