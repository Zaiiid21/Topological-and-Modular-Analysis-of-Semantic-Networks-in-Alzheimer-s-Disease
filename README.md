# Complex Networks A3: Semantic Network Analysis in Alzheimer's Disease

## Project summary
This repository contains the code, data products, and report for the A3 assignment in Complex Networks.  
The analysis compares semantic networks from animal fluency tasks between:

- `NC`: normal controls
- `AD`: participants diagnosed with Alzheimer's disease

Main components include:

- global topological metrics
- small-world analysis using shuffled null models
- community detection (Louvain, DCSBM, Infomap)
- a subject similarity network (SSN)
- an additional transition-network based approach for AD vs NC classification

## Repository layout
- `Notebooks/A3_GroupA3N_Notebook.ipynb`: main Python workflow (feature extraction + modeling)
- `Statistics/A3_GroupA3N_ResultsVisualization.qmd`: R/Quarto visualization and statistical testing
- `Results/Tables/`: generated metric and significance tables
- `Results/Figures/`: generated figures
- `A3_GroupA3N_Report.pdf`: final project report
- `Documentation/Original_Research_Article.pdf`: source paper used as methodological reference
- `Datasets/Networks/`: preprocessed semantic network files (`.pickle`)

## Original dataset (important)
### Provenance (raw source)
- The original semantic fluency corpus comes from the UCSD Shiley-Marcos ADRC and is described in Zemla, J. C., & Austerweil, J. L. (2019), *Complexity*, Article ID 4203158, https://doi.org/10.1155/2019/4203158.
- In that paper, the task is animal semantic fluency (participants list animals), and networks are inferred from list sequences.
- The paper's supplementary materials are referenced at: https://osf.io/j6qea/

### What this repository contains
This repository does **not** contain the original raw fluency tables from NACC/UCSD.  
It contains **preprocessed inferred network files** used for this assignment:

- `Datasets/Networks/ucsd_nc_graphs_usf_persev.pickle`
- `Datasets/Networks/ucsd_ad_graphs_usf_persev.pickle`
- 50 shuffled null-model files for NC
- 50 shuffled null-model files for AD

Total in `Datasets/Networks/`:

- 102 pickle files
- ~759 MB

### Cohort counts in the included pickles
From the two "real" files in this repo:

- `NC`: 97 network entries
- `AD`: 61 network entries
- Total entries: 158

Label mapping note:

- The original paper uses `PAD` (Probable Alzheimer's Disease); this repository uses `AD`.
- The original paper reports 84 `NC` + 41 `PAD` inferred networks in its analysis, while this assignment export contains 97 `NC` + 61 `AD` entries.

Important note:

- `subject` IDs are not unique across groups in these files (19 IDs appear in both `NC` and `AD`), so rows should be treated as diagnosis-specific network entries unless longitudinal linkage is explicitly modeled.

### Pickle schema
Each pickle file contains:

- `subs`: subject IDs
- `subgraphs`: adjacency matrices (one network per entry)
- `items`: dictionary mapping node index -> animal label
- `persev_params`
- `persev_data`
- `numlists`

Shuffled files keep the same schema and correspond to null models.

### Null model used in this project
Following the project report and source methodology, shuffled networks are created by permuting item order within original fluency lists and re-estimating networks. This preserves list-level item frequencies while removing sequential dependencies.

## Reproducibility
### 1) Python notebook workflow
Run:

```bash
jupyter notebook Notebooks/A3_GroupA3N_Notebook.ipynb
```

This generates:

- `Results/Tables/global_network_metrics.csv`
- `Results/Tables/community_metrics.csv`

### 2) R/Quarto statistical analysis and figures
Run:

```bash
quarto render Statistics/A3_GroupA3N_ResultsVisualization.qmd
```

This generates/updates:

- `Results/Figures/global_metrics.png`
- `Results/Figures/small_world_property.png`
- `Results/Figures/community_metrics.png`
- `Results/Figures/community_algorithm_performance.png`
- significance tables in `Results/Tables/`

## Environment
### Python packages used in the notebook
- `numpy`, `pandas`, `matplotlib`, `seaborn`
- `networkx`, `scipy`
- `scikit-learn`
- `infomap`
- `adjustText`
- `graph-tool` (used for DCSBM)

### R packages used in the Quarto file
- `tidyverse`
- `patchwork`

## Notes
- `Datasets/` is listed in `.gitignore`; depending on how you obtained the repo, you may need to place the dataset files manually.
- The assignment report includes additional methodological detail and interpretation: `A3_GroupA3N_Report.pdf`.
