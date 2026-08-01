# IBD T-Cell Marker Analysis

Marker-focused analysis of intestinal CD3+ T cells from patients with Crohn's disease (CD), ulcerative colitis (UC), and non-IBD controls. This repository was created for the C&SBio 10 Group 1 project.

## Research question

Do intestinal T cells from CD, UC, and control samples differ in selected marker patterns, especially in the frequency of cells with a Crohn's-associated tissue-resident memory T-cell phenotype?

The project uses a simplified **CDpop-like** definition:

```
CD4+ CD103+ CD161+ CCR5+ CD27- CCR7-
```

This detected/not-detected rule is an approximation of the original study's gating and clustering procedure.

## Dataset

- **Source:** [GEO GSE218000](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE218000)
- **Study:** [Yokoi et al., PNAS (2023)](https://doi.org/10.1073/pnas.2204269120)
- **Organism/tissue:** human intestinal mucosa
- **Assay:** targeted single-cell RNA expression plus BD AbSeq protein-marker measurements
- **Samples:** 12 total — 4 control, 4 CD, and 4 UC
- **Cells:** 85,524 total — 25,171 control, 27,779 CD, and 32,574 UC
- **Features:** 432 targeted RNA/protein features per sample

Each row represents one cell. Core metadata fields include `Cell_Index`, `Sample`, and `Condition`.

> The raw and processed datasets are not committed to this repository. Obtain source data through GEO or the authorized project Drive and review usage/privacy requirements before adding data.

## Marker panel

| Biological marker | Feature type used |
| --- | --- |
| CD4, CD103, CD161, CD27 | AbSeq protein marker |
| CCR5, CCR7, IFNG, GZMA | Transcript-level feature |

The original paper describes Crohn's-associated CD4+ tissue-resident memory T cells as enriched for CCR5, CD161, CD127, CD69, and CCR6 and reduced for CCR7 and CD27.

## Analysis workflow

1. Load the 12 compressed sample files, skip six metadata rows, and use `Cell_Index` as the cell identifier.
2. Add sample and condition labels, check dimensions and missing values, and concatenate samples vertically.
3. Parse feature names into marker, gene, probe ID, and measurement type.
4. Summarize cell counts by sample and condition and calculate basic quality-control measures.
5. Apply `log1p` transformations to skewed marker counts.
6. Compare selected markers across conditions at cell and sample levels.
7. Perform differential-expression analysis and generate volcano plots and heatmaps.
8. Define the simplified CDpop-like subset and compare its frequency by sample.
9. Apply pairwise Wilcoxon tests and Benjamini-Hochberg multiple-testing correction where appropriate.
10. Explore PCA and other dimensionality-reduction views.

## Main findings

- CD and UC samples showed different expression patterns relative to controls.
- CD samples showed a higher observed frequency of CDpop-like cells than control and UC samples, with substantial between-patient variation.
- CD103 and CD161 positivity appeared lower in UC, while CCR7 positivity appeared higher.
- GZMA and IFNG varied across samples; CD4 and CD27 were detected in most cells.
- Sample-level marker comparisons were not statistically significant after Benjamini-Hochberg correction. These results should therefore be treated as descriptive trends, not confirmatory evidence.

## Limitations

- Only four patient samples were available per condition.
- Cells from the same patient are not independent; sample-level inference is preferred.
- The simplified positive/negative marker threshold does not reproduce the source study's full gating or clustering.
- Protein and transcript measurements are mixed across the selected panel.
- Cell-level tests can produce very small p-values because of the large number of cells; biological effect sizes and patient-level replication remain essential.
- Unequal cell counts across samples and conditions may affect summaries.

## Project artifacts identified in Drive

### Code

- `EasonLuo_DataPreprocessing.ipynb`
- `AlexYoung-box, bar, and heatmap.ipynb`
- `PreProcessing_Volcanoplots_Heatmaps_Isabella.ipynb`
- `JiayanLi_CDpop_marker_analysis.Rmd`

### Documentation and outputs

- C&SBio Dataset / project outline
- C&SBio 10 Presentation
- `Presentation.pdf`
- `FIGURES.pdf`
- Original and preprocessed dataset folders
- Project `README.txt`

## Contributors

C&SBio 10 Group 1: Isabella Chavez, Jiayan Li, Eason Luo, and Alex Young.
