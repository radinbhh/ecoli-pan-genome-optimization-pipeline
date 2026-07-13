Here is your **polished README**, updated to reflect the final state of your project. I have:

- Removed the outdated "Conservation Logo" and replaced it with the **Functional Breakdown bar chart**.
- Added a **"A Note on Data Quality"** section to document the *Burkholderia/Shewanella/Streptococcus* incident.
- Adjusted the "Feature Comparison" table to reflect what you actually implemented (rolling hash instead of LSH, functional categorisation instead of MSA).
- Added a **"Key Biological Findings"** summary to the Overview.
- Updated the "Results Preview" section with your actual final figures.

---

### Polished README (Full Version)

```markdown
# ecoli-pan-genome-optimization-pipeline

A comparative genomics pipeline using MinHash, rolling hash optimisation, bipartite matching, and functional categorisation to analyse 8 *E. coli* strains — from raw genomes to phylogenetic trees, synteny dot-plots, and core genome functional breakdowns.

**Author:** Radin Baharsefat  
**Course:** Bioinformatics Algorithms Portfolio Project — Sharif University of Technology  
**Instructor:** Dr. Somayyeh Koohi  
**Date:** Spring 2026  
**License:** MIT

---

## Overview

This repository contains a comparative genomics pipeline implemented from first principles. Given eight complete *Escherichia coli* genomes, the pipeline addresses three core questions:

1. **How similar are these strains at the whole-genome level?**  
   Addressed using MinHash with rolling hash optimisation for fast Jaccard distance estimation.

2. **Which genes are truly orthologous across strains?**  
   Addressed using weighted bipartite matching to identify optimal one-to-one gene matches.

3. **What do these conserved genes actually do?**  
   Addressed by parsing functional annotations and categorising the core genome into biological roles (metabolism, translation, transport, etc.).

Unlike standard tools such as `sourmash` or `OrthoFinder`, this project implements the core mathematical procedures manually — demonstrating algorithmic understanding rather than tool usage alone.

---

## Key Biological Findings

Running the pipeline on eight *E. coli* strains revealed:

- **K-12 MG1655 and HS (commensal)** are genomically close (Jaccard distance ~0.39) — both are non-pathogenic.
- **K-12 and O157:H7 Sakai** share **3,613 orthologous genes** — roughly 84% of the K-12 genome — confirming a highly conserved core genome despite their dramatically different lifestyles.
- **O111 and O26** were flagged as outliers — not a pipeline failure, but a consequence of their draft assemblies (missing ~50% of genomic content).
- The functional breakdown of the 3,613 conserved orthologs showed:
  - **40.0% hypothetical proteins** — conserved, essential, but uncharacterised.
  - **27.0% metabolic enzymes** — glycolysis, TCA cycle, amino acid synthesis.
  - **10.5% transporters** — nutrient uptake and efflux.
  - **7.8% regulatory proteins** — transcription factors, sigma factors.
  - The remainder: membrane proteins, DNA replication machinery, ribosomal proteins (overrepresented, confirming extreme purifying selection), chaperones, and cell division proteins.

The large "unknown" category is a humbling reminder: even in the most studied organism on Earth, we still have much to learn.

---

## A Note on Data Quality

I ran into an unexpected issue early on: several of the accessions I initially used pointed to completely different organisms — *Burkholderia lata*, *Shewanella baltica*, and *Streptococcus equi* — instead of *E. coli*. A quick header check of the downloaded FASTA files revealed the problem, and I switched to manually verified URLs for all strains.

This was a valuable lesson: **always validate your data.** A five‑second sanity check saved me from wasting hours analysing the wrong organisms. This experience reinforced the importance of quality control in any computational biology pipeline.

---

## Algorithms Used

The pipeline is structured into five sequential steps:

| Step | Algorithm | Purpose |
| :--- | :--- | :--- |
| **1. Data Acquisition** | Automated NCBI retrieval | Fetches genomes and annotations from RefSeq. |
| **2. Genome Similarity** | MinHash + Rolling Hash | Estimates whole-genome Jaccard distances without brute-force alignment. |
| **3. Phylogenetics** | UPGMA (hierarchical clustering) | Builds a rooted evolutionary tree from the distance matrix. |
| **4. Orthology** | Maximum Weight Bipartite Matching | Identifies optimal one-to-one gene matches between two strains. |
| **5. Functional Categorisation** | GFF parsing + keyword classification | Reveals what the conserved genes actually do — metabolism, translation, transport, and more. |

The core algorithms (MinHash, bipartite matching, and Needleman-Wunsch) are written from scratch in Python — because understanding how they work is more important than just calling a library.

---

## Folder Structure

```text
ecoli-pan-genome-optimization-pipeline/
│
├── data/
│   ├── raw/                 # Downloaded genomes (.fna, .gff)
│   ├── processed/           # Extracted genes, intermediate outputs
│   └── results/             # Final matrices and ortholog lists
│
├── notebooks/
│   └── 01_pan_genome_pipeline.ipynb   # Master Jupyter notebook
│
├── src/                     # Reusable Python modules
│   ├── sketching.py         # MinHash and rolling hash functions
│   ├── matching.py          # Bipartite matching solvers
│   └── visualization.py     # Plotting helpers
│
├── figures/                 # Exported images for README
│   ├── minhash_distance_heatmap.png
│   ├── phylogenetic_tree.png
│   ├── synteny_dotplot.png
│   └── functional_breakdown.png
│
├── requirements.txt         # Python dependencies
├── LICENSE                  # MIT License
└── README.md                # This file
```

---

## Data

**Organisms:** Eight complete strains of *Escherichia coli*:

| Strain | Description |
| :--- | :--- |
| K-12 MG1655 | Lab strain (non-pathogenic) |
| O157:H7 Sakai | Enterohemorrhagic pathogen |
| O104:H4 | Outbreak strain (Germany 2011) |
| O111:H | Enteropathogenic pathogen |
| O26:H11 | Enterohemorrhagic pathogen |
| O103:H2 | Enterohemorrhagic pathogen |
| O145:H28 | Enterohemorrhagic pathogen |
| HS | Commensal isolate from a healthy human |

**Source:** NCBI RefSeq (accessions are listed in `data/raw/sources.txt`).  
**Total size:** Approximately 40 MB, selected for lightweight execution on standard hardware.

The Jupyter notebook includes an automatic downloader using `requests` and Biopython; no manual file retrieval is required.

---

## Setup and Execution

1. **Clone the repository:**
   ```bash
   git clone https://github.com/radinbhh/ecoli-pan-genome-optimization-pipeline.git
   cd ecoli-pan-genome-optimization-pipeline
   ```

2. **Install dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

3. **Run the Jupyter notebook:**
   ```bash
   jupyter notebook notebooks/01_pan_genome_pipeline.ipynb
   ```

4. **Execute all cells sequentially.** The notebook will:
   - Download the 8 genomes automatically.
   - Compute whole-genome distances and build a phylogenetic tree.
   - Identify orthologs and generate a synteny dot-plot.
   - Categorise the conserved core genome by function.
   - Save all figures to the `figures/` folder.

---

## Dependencies

The pipeline requires Python 3.9 or higher. All dependencies are versioned in `requirements.txt`:

- `biopython` – genome parsing and alignment.
- `numpy`, `scipy`, `pandas` – numerical computing.
- `networkx` – bipartite matching.
- `matplotlib`, `seaborn` – visualisations.
- `scikit-learn` – UPGMA clustering.
- `requests` – automatic data download.

---

## Feature Comparison

| Aspect | Standard Approach | This Pipeline |
| :--- | :--- | :--- |
| Whole-genome distance | Run `sourmash` or `fastANI` | Self-implemented MinHash + rolling hash |
| Ortholog detection | Use `OrthoFinder` (black-box) | Custom bipartite matching |
| Functional analysis | Manual or use `eggNOG` | GFF parsing + keyword classification |
| Pedagogical focus | Tool execution | Algorithm implementation |
| Resume emphasis | Familiarity with software | Independent computational problem-solving |

---

## Results Preview

The following figures are generated after a complete pipeline run and exported to the `figures/` folder:

### Figure 1: MinHash Distance Heatmap
**File:** `figures/minhash_distance_heatmap.png`  
A heatmap showing pairwise Jaccard distances between all eight strains. Lower values (darker) indicate higher genomic similarity. K-12 and HS cluster closely, while O111 and O26 appear as outliers due to their draft assembly status.

### Figure 2: Phylogenetic Tree
**File:** `figures/phylogenetic_tree.png`  
A UPGMA dendrogram derived from the distance matrix. Complete genomes cluster together, while the draft assemblies (O111, O26) branch off as outliers — demonstrating the pipeline's sensitivity to data quality.

### Figure 3: Synteny Dot-Plot
**File:** `figures/synteny_dotplot.png`  
A dot-plot comparing K-12 and O157:H7. The strong diagonal line confirms high synteny (gene order conservation). Off-diagonal clusters indicate genomic rearrangements or pathogenicity islands unique to O157.

### Figure 4: Functional Breakdown of Conserved Orthologs
**File:** `figures/functional_breakdown.png`  
A bar chart showing the functional categories of the 3,613 conserved orthologs. Metabolic enzymes dominate (27%), followed by transporters (10.5%), regulatory proteins (7.8%), and ribosomal proteins (1.9%, overrepresented relative to the genome). The 40% "Others" category — hypothetical proteins — highlights how much remains to be discovered, even in *E. coli*.

---

## Future Extensions

- Extend the pipeline to **all-vs-all gene comparisons** across all 8 strains to define the pan-genome (core, accessory, and unique genes).
- Integrate **linear programming (LP)** to model gene gain/loss events more precisely.
- Apply this pipeline to other bacterial species (e.g., *Salmonella* or *Pseudomonas*) to test its generalisability.
- Dig deeper into the 40% hypothetical proteins — since they are conserved, they are likely essential, and characterising them would be a meaningful research direction.

---

## References

- Broder, A. Z. (1997). *On the resemblance and containment of documents.* (MinHash foundation).
- Gusfield, D. (1997). *Algorithms on Strings, Trees, and Sequences.* (Background on suffix trees and MSA).
- Edmonds, J., & Karp, R. M. (1972). *Theoretical improvements in algorithmic efficiency for network flow problems.* (Min-cost flow theory).

---

## Contact

- GitHub: [https://github.com/radinbhh](https://github.com/radinbhh)
- LinkedIn: [www.linkedin.com/in/radin-baharsefat-878639231](www.linkedin.com/in/radin-baharsefat-878639231)
```

---