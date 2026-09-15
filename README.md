RNA-Seq Differential Expression Analysis in Galaxy
End-to-end analysis of a published Drosophila melanogaster dataset, built entirely in Galaxy, no local compute or code required.

**Overview**
This project reproduces a full RNA-seq differential expression pipeline - from raw sequencing reads to biologically interpretable results - using [Galaxy](https://usegalaxy.org), the open-source, web-based platform for reproducible computational biology.

**Biological question:** *Pasilla*, the *Drosophila* homolog of the human splicing regulators NOVA1/NOVA2, was experimentally knocked down via RNAi. Which genes change expression as a result, and what biological processes do they belong to?

**Why this project:** it demonstrates the complete RNA-seq analysis skillset — quality control, read trimming, spliced alignment, gene-level quantification, differential expression statistics, and functional enrichment — using an industry-standard platform, on a real published dataset, with every step fully reproducible and shareable.

---

## Dataset
| **Study** | Brooks et al. (2011), *Genome Research* — *Pasilla* knockdown in *Drosophila melanogaster* |
| **GEO accession** | [GSE18508](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE18508) |
| **Samples used in this analysis** | 2 of 7 available replicates: `GSM461177` (untreated/control) and `GSM461180` (Pasilla-treated) |
| **Data source** | [Zenodo record 6457007](https://zenodo.org/record/6457007) |
| **Reference genome** | *D. melanogaster* dm6 (Galaxy built-in index) |
| **Annotation** | `Drosophila_melanogaster.BDGP6.32.109_UCSC.gtf` |

> **Note on scope:** this analysis uses 2 samples (1 per condition) to demonstrate the full pipeline end-to-end. A statistically robust differential expression analysis requires biological replicates — the complete study has 4 untreated + 3 treated replicates. Scaling this pipeline to all 7 samples is straightforward using the reusable Galaxy workflow extracted from this history (see **Reproducing this analysis** below).

## Pipeline
Raw FASTQ reads
      │
      ▼
Quality control (Falco + MultiQC)
      │
      ▼
Read trimming (Cutadapt)
      │
      ▼
Spliced alignment (RNA STAR)
      │
      ▼
Gene-level counting (featureCounts)      ← in progress
      │
      ▼
Differential expression (DESeq2)         ← in progress
      │
      ▼
Functional enrichment (GO / KEGG)        ← in progress

### 1. Quality control
- **Tools:** Falco (FastQC-compatible), MultiQC
- Verified read quality, GC content, duplication levels, and adapter content across all 4 raw FASTQ files (2 samples × forward/reverse) before proceeding.

### 2. Read trimming
- **Tool:** Cutadapt
- Parameters: minimum length = 20bp, quality-based filtering, paired-end mode (drops both mates of a pair if either read falls below the length cutoff).
- Re-ran MultiQC on Cutadapt's reports to confirm trimming improved read quality.

### 3. Alignment
- **Tool:** RNA STAR (Galaxy version 2.7.11b+galaxy0) — a splice-aware aligner, required because RNA-seq reads span exon-exon junctions after intron removal.
- Reference: built-in dm6 index, with the Ensembl GTF supplied for splice-junction annotation.
- Key parameters: junction overhang length = 36 (read length − 1), MAPQ for unique mappers = 60 (recommended for modern downstream tools over STAR's legacy default of 255).
- Run as a **paired-end collection** (not individual datasets) to ensure Galaxy correctly matches each sample's forward and reverse reads as true pairs rather than aligning them independently.

### 4. Gene-level quantification *(in progress)*
- **Tools:** RSeQC (Infer Experiment, for library strandedness) → featureCounts
- > **TODO:** fill in once complete — strandedness result, total genes with non-zero counts.

### 5. Differential expression *(in progress)*
- **Tool:** DESeq2
- > **TODO:** fill in once complete:
> - Number of genes tested
> - Number of significant DE genes (adjusted p-value < 0.05)
> - Number up-regulated vs. down-regulated
> - Volcano plot, PCA plot, and heatmap (add as images in `figures/`, link them here)

### 6. Functional enrichment *(in progress)*
- **Tool:** goseq (GO term enrichment), with gene-length correction
- > **TODO:** fill in once complete — top enriched GO terms/pathways and what they suggest biologically (expect splicing/RNA-processing-related terms, given *Pasilla*'s known function).

## Repository structure

rnaseq-galaxy-pasilla-project/
├── README.md                  ← this file
├── figures/                   ← QC plots, volcano plot, PCA plot, heatmap (add as you generate them)
├── results/
│   ├── deseq2_results.tsv     ← full DESeq2 output table (add once generated)
│   ├── significant_genes.tsv  ← filtered significant genes (add once generated)
│   └── go_enrichment.tsv      ← GO/KEGG enrichment results (add once generated)
├── workflow/
│   └── rnaseq-pipeline.ga     ← exported Galaxy workflow (add once extracted — see below)
└── LICENSE

## Reproducing this analysis

The entire analysis was performed on [usegalaxy.org](https://usegalaxy.org) using only the web interface — no local installation or coding required.

1. **View the exact steps interactively:** this project's full Galaxy history is published and shareable — every tool, parameter, and intermediate file is inspectable.
   > **TODO:** publish your history (History menu → *Share or Publish*) and paste the public link here.
2. **Re-run it yourself:** the pipeline above was extracted as a reusable Galaxy workflow.
   > **TODO:** extract your workflow (Workflow menu → *Extract from history*), export it as a `.ga` file, add it to `workflow/`, and link it here.
3. **Scale to all 7 replicates:** re-run the same workflow on the remaining 5 samples from [GSE18508](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE18508) / [Zenodo 6457007](https://zenodo.org/record/6457007) for a statistically complete differential expression analysis.

## Tools used

| Tool | Purpose | Version |
|---|---|---|
| Falco | Sequence quality control | 1.3.2+galaxy0 |
| MultiQC | Aggregate QC reports | 1.35+galaxy4 |
| Cutadapt | Adapter/quality trimming | — |
| RNA STAR | Spliced read alignment | 2.7.11b+galaxy0 |
| featureCounts | Gene-level read counting | — |
| DESeq2 | Differential expression statistics | — |
| goseq | GO enrichment (length-bias corrected) | — |

## Key skills demonstrated

- NGS quality control and interpretation
- Read trimming and adapter/quality filtering
- Splice-aware alignment and understanding of BAM file structure
- Strandedness determination and gene-level read quantification
- Differential expression statistics and multiple-testing correction
- Functional/pathway enrichment analysis
- Reproducible, shareable computational workflow design (Galaxy histories & workflows)

## References

- Brooks AN, Yang L, Duff MO, et al. (2011). Conservation of an RNA regulatory map between *Drosophila* and mammals. *Genome Research*, 21(2), 193–202.
- Love MI, Huber W, Anders S (2014). Moderated estimation of fold change and dispersion for RNA-seq data with DESeq2. *Genome Biology*, 15, 550.
- Ewels P, Magnusson M, Lundin S, Käller M (2016). MultiQC: summarize analysis results for multiple tools and samples in a single report. *Bioinformatics*, 32(19), 3047–3048.
- Galaxy Training Network — [Reference-based RNA-Seq data analysis tutorial](https://training.galaxyproject.org/training-material/topics/transcriptomics/tutorials/ref-based/tutorial.html)

## License

This project is licensed under the MIT License — see [LICENSE](LICENSE) for details.
