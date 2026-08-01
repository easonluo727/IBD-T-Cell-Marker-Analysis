IBD T-Cell Marker Analysis

PROJECT OVERVIEW
This project analyzed a targeted single-cell RNA sequencing and AbSeq dataset generated from human intestinal CD3+ T cells in a study of inflammatory bowel disease.

RESEARCH QUESTION
Do intestinal T cells from Crohn’s disease, ulcerative colitis, and control samples differ in selected marker patterns, especially in the frequency of cells with a CDpop-like phenotype?

The simplified CDpop-like phenotype used in this project is:
CD4+ CD103+ CD161+ CCR5+ CD27- CCR7-

DATASET STRUCTURE
Each row represents one cell.

Important metadata columns:
- Cell_Index: cell identifier
- Sample: patient/sample identifier
- Condition: Control, CD, or UC

ANALYSIS WORKFLOW
- Loaded and combined the single-cell datasets.
- Added and cleaned sample and condition labels.
- Summarized the number of cells by sample and condition.
- Calculated basic cell-level quality-control measurements.
- Examined overall and selected marker-expression patterns.
- Applied log transformations to highly skewed feature counts.
- Compared marker patterns across Control, CD, and UC.
- Performed differential-expression analyses and generated volcano plots and heat map.
- Defined a simplified CDpop-like cell population.
- Compared CDpop-like cell frequency across conditions at the sample level.
- Used statistical testing and multiple-testing correction where appropriate.



CODE FILES
EasonLuo_DataPreprocessing.ipynb
- Load multiple compressed CSV files; skipping metadata rows
- Add sample and condition identifier columns
- Check for missing values and data dimensions
- Concatenate the UC, CK, and CD datasets
- Create scatter plots comparing the single-cell expression levels of two protein markers

Main Packages: pandas, matplotlib

AlexYoung-box, bar, and heatmap.ipynb
- Bar plot of number of cells per condition
- Bar plot of number of cells per sample
- Boxplot with log-transformed CD data
- Boxplot with log-transformed CD data, filtered >200 and <100000
- Heatmap of all gene expression by condition
- Heatmap of gene expression >50 by condition
- Heatmap for marker expression by condition
- Boxplot of CD4 expression by condition (log-transformed)
- Boxplot of CD27 expression by condition (log-transformed)
- Boxplot of CD28 expression by condition (log-transformed)

Main packages:
I used numpy, pandas, and matplotlib. Numpy was to handle the numbers of the data. It also allowed me to do the log transformations. Pandas was to handle the data as a dataframe. Matplotlib was to create the graphs and plots that I did.


JiayanLi_CDpop_marker_analysis.Rmd
- loads pre-processed dataset
- cleans condition labels to Control, CD, and UC
- computes basic QC calculations
- sample summaries
- selects the marker panel CD4, CD103, CD161, CCR5, CCR7, CD27, IFNG, and GZMA
- applies log1p transformation to selected marker values
- marker positivity analysis
- performs pairwise Wilcoxon tests for each marker using sample-level values
- applies BH correction across all marker and condition comparisons
- performs PCA using marker panel
- defines CDpop-like cells using the simplified phenotype: CD4+ CD103+ CD161+ CCR5+ CD27- CCR7-
- calculates the percentage of CDpop-like cells within each sample
- creates a sample-level boxplot comparing CDpop-like frequency across conditions

Main Packages:
- dplyr: for data cleaning, grouping cells by sample/ condition, creating summary tables, and calculating marker positivity
- tidyr: reshaping dataframes for plotting
- ggplot2: for creating figures 


IsabellaChavez_PreProcessing_Volcanoplots_Heatmap.ipynb
==========FOR PYTHON CD PREPROCESSING=================================
- Load in each file
- Label each file a different file path
- Read in file using file path skipping first 6 rows
- Renames each file to a different CD sample 
    - CD_df_1; CD_df_2; CD_df_3; CD_df_4
- Insert a column that displays sample name for all 4 df
- Concatenate df together vertically
- Check shape of each df and compare before and after concatenating
- Check for any N/A values
- Add one last column to concatenated chrons df labeled condition
- Fill column with “Chron’s_Disease

Packages: 
- Pandas: concatinate
- Json: unzipping files
==========FOR VOLCANO PLOTS============================================
- Load in UC_CK_CD dataset
- Drop 1st column and split DF into metagenome and gene dataset
- Change gene labeled names for aesthetics
- sum all gene counts horizontally to yield total gene expression
- divide each original gene counts by total cell count
- multiply by a 1,000,000 to change into count per million
- log transform matrix so that we get 10^n and add 1
- Group labels by condition
- Initialize df used for plotting
- Calculate log2fold change for CD vs Control
    - Calculate mean expression of each gene in CD group using log2fold df
    - Subtract mean from control group
    - Use log2fold valued for CD for plotting
- Calculate p val for CD vs Control
    - Use stats t test to return p value
    - Map p values to gene names and transform p values using log 2 fold
- Calculate log2fold change for UC vs Control
    - Calculate the mean expression of each gene UC group using log2fold df
    - Subtract mean from control group 
    - Use log2fold valued for UC for plotting
- Calculate p val for UC vs Control
    - Use stats t test to return p value
    - Map p values to gene names and transform p values using log 2 fold
- adjust log2 fold change and p value threshold to yield significant DEG
- Subset and color-code up reg./down reg./not sig/ genes based on threshold values
- Use plotly express package to plot CD vs control and  UC vs control
- Save images

Main Packages:
- SciPy: calculate p values
- Numpy: calculate log2fc
- Pandas: panda series, div, sum
==========FOR HEATMAP==================================================
- From volcano plot, create list of genes that exceeded log2fold and pval threshold
- Take the mean expression of all cells for each group
- Subtract control means from entire df of means
- Order the groups displayed in heatmap 
- Plot heatmap using seaborn package clustermap

Main packages:
- Pandas: panda series, div, sum, loc, mean
- Seaborn: clustermap used for heatmap

MAIN FINDINGS
- Observed different genes expressed in CD and UC samples compared to Control samples 
- CD samples showed a higher frequency of CDpop-like cells than Control and UC samples
- Sample-level pairwise marker comparisons were not statistically significant after Benjamini-Hochberg correction


LIMITATIONS
- The dataset only contains four patient samples per condition
- Some analyses used protein-marker features, while others used transcript-level features
- P values were significant due to sample size making it harder to interpret data 
- The simplified CDpop-like definition used detected versus undetected marker signal rather than the original study’s full gating or clustering procedure