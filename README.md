# RNA_Seq_Galaxy_Bioinformatics_Project

### My RNA-Seq Differential Expression Analysis Journey (2026)
**Learning in Public • Hands-On Bioinformatics • India**

This repository documents a complete, end-to-end RNA-seq differential expression analysis. I built entirely using [Galaxy](https://usegalaxy.org), the open-source, web-based platform for computational biology - no local installation or coding required. It reproduces a real published study on *Drosophila melanogaster* (fruit fly), from raw sequencing reads all the way to biological interpretation.

## The Project: RNA-Seq Analysis of Pasilla Gene Knockdown
**Status: CORE PIPELINE 100% COMPLETE** as of 17 Sep 2026

**Biological question:** *Pasilla*, the fly counterpart of human splicing regulators NOVA1/NOVA2, was experimentally knocked down. Which genes change expression as a result, and what does that tell us biologically?

| Stage | Tool(s) | Status |
|---|---|---|
| 1. Data collection | Zenodo / GEO (GSE18508) | Completed |
| 2. Quality control | Falco, MultiQC | Completed |
| 3. Read trimming | Cutadapt | Completed |
| 4. Spliced alignment | RNA STAR | Completed |
| 5. Gene quantification | featureCounts | Completed |
| 6. Differential expression | Custom fold-change analysis (SQL/Query Tabular) | Completed |
| 7. Visualization | ggplot2 scatterplot | Completed |
| 8. Functional enrichment | goseq (GO/KEGG) | Attempted; not supported for this genome/ID combination on this Galaxy instance |

**Total hands-on time:** ~1 full day of active troubleshooting and analysis
**Samples analyzed:** 2 (1 untreated control, 1 Pasilla-knockdown), from a 7-sample published dataset

### What I Learned & Built
- Full command of the Galaxy platform: collections, paired-end data handling, workflow extraction, and history publishing
- Hands-on experience with the complete RNA-seq pipeline: QC, trimming, splice-aware alignment, gene counting, differential expression, and enrichment
- Real-world debugging: diagnosed and worked around a genuine statistical limitation (DESeq2 and edgeR both require biological replicates to estimate dispersion, and this pilot dataset had none) by building a defensible custom fold-change analysis using SQL-based filtering (Query Tabular) after Galaxy's classic Filter tool proved unreliable
- Understanding of why methodological transparency matters: this README documents not just the results, but the constraints and workarounds behind them
- Practical experience with reproducible science: this entire analysis can be re-run from a single published Galaxy history link (below)

### Key Results

- **Background gene set:** 552 genes passed low-count filtering (expressed above baseline in at least one sample)
- **Genes with 2-fold or greater expression change:** see `significant_genes_final` in this repo for the exact count and full list
- **Top down-regulated genes (higher in untreated / lower after Pasilla knockdown):**
  - FBgn0039827 (log2FC approximately -4.63)
  - FBgn0039155 (log2FC approximately -4.41)
  - FBgn0024288 (log2FC approximately -4.19)
  - FBgn0085359 (log2FC approximately -3.97)
  - FBgn0264753 (log2FC approximately -3.72)
- **Top up-regulated genes (higher in treated / higher after Pasilla knockdown):**
  - FBti0019405 (log2FC approximately 2.45)
  - FBgn0263986 (log2FC approximately 2.22)
  - FBti0019466 (log2FC approximately 1.66)
  - FBgn0265276 (log2FC approximately 1.65)
  - FBgn0037678 (log2FC approximately 1.58)

### An Honest Note on Method

This analysis used only 1 sample per condition rather than full biological replicates. Formal statistical tools (DESeq2, edgeR) require replicates to estimate biological variability and refused to run on this design — a real, well-documented limitation, not a bug. Rather than force an inappropriate statistical test, results here are presented as **fold-change rankings** on a low-count-filtered gene set, consistent with standard practice for unreplicated pilot RNA-seq data.

### Files in This Repository
- `README.md` — this file
- `Galaxy-Workflow-RNA-seq_Pasilla_Pipeline.ga` — the complete, reusable Galaxy workflow
- Result tables (gene counts, fold-change values, significant gene list)
- Fold-change scatter plot (PNG)

### Reproduce This Analysis
- **Full Galaxy history (every step, every parameter):** https://usegalaxy.org/u/ayesha_shk/h/copy-of-rna-seq-pasilla-project
- **Reusable workflow file:** `Galaxy-Workflow-RNA-seq_Pasilla_Pipeline.ga` (in this repo)
- **Source dataset:** [GSE18508](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE18508) / [Zenodo 6457007](https://zenodo.org/record/6457007)

### What's Next
Planning to extend this analysis to the full 7-sample dataset (4 untreated + 3 treated replicates) using the extracted Galaxy workflow, which would enable proper DESeq2/edgeR statistical testing rather than the fold-change workaround used here.

More bioinformatics projects coming as I keep building hands-on skills with Galaxy and beyond.

#Bioinformatics #RNAseq #Genomics #Galaxy #ComputationalBiology #LearningInPublic #WomenInSTEM #Python
