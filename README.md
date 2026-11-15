# DeepRNAi public dataset and model evaluation

This repository contains the DeepRNAi public dataset, comprising 240 siRNAs across the training and test splits, and a Jupyter notebook for comparing model performance on the public test set.

## Repository contents

- **`examples/evaluations.ipynb`**: Example notebook showing how to load in the provided dataset and compare the performance of mini-DeepRNAi, mini-DeepRNAi without biological foundation model (BioFM) embeddings, and mini-Random Forest (Figure S5 in the [preprint](https://www.biorxiv.org/content/10.64898/2026.11.12.766311))
- **`data/deeprnai_dataset.csv.gz`**: Compressed CSV containing the public training and test data, experimental measurements, and model predictions
- **`environment.yml`**: YAML file to create a conda environment that will work with the example notebook

## Dataset

Each row represents an siRNA-readout gene pair, linking an siRNA to the measured expression change of a readout gene. Each siRNA therefore appears in multiple rows.

The `target_gene_name` column identifies the intended target of the siRNA, whereas `readout_gene_name` identifies the gene whose expression was measured. The `split` column specifies the predefined training or test assignment.

### Data columns

| Column | Description |
| --- | --- |
| `sirna_id` | Identifier for the siRNA. |
| `sirna_guide_seq` | Guide-strand nucleotide sequence. |
| `sirna_synthesis` | Synthesis identifier or annotation for the siRNA. |
| `readout_gene_name` | Gene symbol of the gene whose expression is measured as the readout after siRNA treatment. |
| `readout_transcript_id` | Transcript ID (`ncbi_refseq.v110`) of the transcript selected to represent the readout gene.  |
| `target_gene_name` | Intended target gene of the siRNA. |
| `target_transcript_id`| Transcript ID (`ncbi_refseq.v110`) of the transcript selected to represent the target gene. |
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

df = pd.read_csv("data/deeprnai_dataset.csv.gz")

train_df = df.loc[df["split"] == "train"].copy()
test_df = df.loc[df["split"] == "test"].copy()
```

### Environment setup

An environment file to create a conda environment that can be used with the notebook has been provided:

```bash
conda env create -f environment.yml
conda activate deeprnai-evaluation
```

## Preprint and citation

The DeepRNAi preprint is available on bioRxiv: 
[DeepRNAi: Predicting on- and off-target siRNA activity and establishing a benchmark](https://www.biorxiv.org/content/10.64898/2026.11.12.766311)

If you use this public dataset in your research, please cite:

```
@article{Masse2026DeepRNAi,
    author = {Masse, Nicolas Y. and Lau, Tammy T.Y. and Wintersinger, Jeff and Slobodyanyuk, Mykhaylo and Li, Roujia and Chu, Vivian S. and Zhu, Helen and Kanuparthi, Bhargav and La, Jonathan and Fischbacher, Brodie and Arseneault, Krista and Johnson, Sydney and Lu, Wang-Ting and Lorincz-Comi, Noah and Bohn, Emma and Cole, Christopher B. and Hoffman, Gregory R. and Lazar, Nathan H. and Findlay, Scott D. and Celaj, Albi and Frey, Brendan J.},
    title = {DeepRNAi: Predicting on- and off-target siRNA activity and establishing a benchmark},
    year = {2026},
    journal = {bioRxiv},
    doi = {10.64898/2026.11.12.766311},
    url = {https://www.biorxiv.org/content/10.64898/2026.11.12.766311}
}
```

## License

This dataset is made available under the Creative Commons Attribution 4.0 International Public License.
