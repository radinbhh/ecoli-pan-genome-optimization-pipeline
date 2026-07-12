# ecoli-pan-genome-optimization-pipeline
A comparative genomics pipeline using MinHash, LSH, Bipartite Matching, and Min-Cost Max-Flow to analyze 8 E. coli strains — from raw genomes to phylogenetic trees, synteny dot-plots, and conservation logos.

**Author:** Radin Baharsefat 
**Course:** Bioinformatics Algorithms Portfolio Project - Sharif University of Technology
**Instructor:** Dr. Somayyeh Koohi
**Date:** Spring 2026
**License:** MIT

---

## Overview

This repository contains a comparative genomics pipeline implemented from first principles. Given eight complete *Escherichia coli* genomes, the pipeline addresses two primary biological questions:

1.  How similar are these strains at the whole-genome level?  
    This is addressed using MinHash and Locality-Sensitive Hashing (LSH) for fast distance estimation.

2.  Which genes are true orthologs across strains, including cases where duplications exist?  
    This is addressed using weighted bipartite matching, upgraded to a min-cost max-flow network to handle one-to-many and many-to-many relationships.

Unlike standard tools such as `sourmash` or `OrthoFinder`, this project implements the core mathematical procedures manually to demonstrate algorithmic understanding rather than tool usage alone.

---

## Algorithms Used

The pipeline is structured into six sequential steps:

- **Step 1: Genome Sketching**  
  *Algorithm:* MinHash + LSH  
  *Purpose:* Estimate Jaccard similarity between whole genomes in linear time.

- **Step 2: Phylogenetic Projection**  
  *Algorithm:* Neighbor-Joining / t-SNE (via scikit-learn)  
  *Purpose:* Visualize strain relationships as a tree or two-dimensional projection.

- **Step 3: Gene Similarity Matrix**  
  *Algorithm:* Local BLAST (via Biopython)  
  *Purpose:* Compute pairwise similarity scores between all coding sequences of two strains.

- **Step 4: Ortholog Mapping (Core)**  
  *Algorithm:* Weighted Bipartite Matching → Min-Cost Max-Flow  
  *Purpose:* Identify optimal one-to-one orthologs. The flow network extension allows splitting matches when gene duplications (paralogs) are present.

- **Step 5: Synteny Visualization**  
  *Algorithm:* Dot-plot generation  
  *Purpose:* Detect chromosomal rearrangements, inversions, and translocations.

- **Step 6: Conservation Analysis**  
  *Algorithm:* Multiple Sequence Alignment (MSA) + Sequence Logo  
  *Purpose:* Identify highly conserved functional domains across all strains.

---

## Folder Structure

```text
ecoli-pan-genome-optimization-pipeline/
│
├── data/
│   ├── raw/                 # Downloaded genomes (.fna, .gff)
│   ├── processed/           # Extracted genes, BLAST outputs
│   └── results/             # Final matrices and ortholog lists
│
├── notebooks/
│   └── 01_pan_genome_pipeline.ipynb   # Master Jupyter notebook
│
├── src/                     # Reusable Python modules
│   ├── sketching.py         # MinHash and LSH functions
│   ├── matching.py          # Bipartite and MCMF solvers
│   └── visualization.py     # Plotting helpers
│
├── figures/                 # Exported images for README
│   ├── phylogenetic_tree.png
│   ├── synteny_dotplot.png
│   └── conservation_logo.png
│
├── requirements.txt         # Python dependencies
└── README.md                # This file
```

---

## Data

Organisms: Eight complete strains of Escherichia coli (e.g., K-12 MG1655, O157:H7, O104:H4).

Source: NCBI RefSeq (accessions are listed in data/raw/sources.txt).

Total size: Approximately 40 MB, selected for lightweight execution on standard hardware.

The Jupyter notebook includes an automatic downloader using requests and Biopython; no manual file retrieval is required.

---

## Setup and Execution

- 1. Clone the repository:
   ```cmd
   git clone https://github.com/your-username/ecoli-pan-genome-optimization-pipeline.git
   cd ecoli-pan-genome-optimization-pipeline
   ```

- 2. Install dependencies:
   ```cmd
   pip install -r requirements.txt
   ```

- 3. Run the Jupyter notebook:
   ```cmd
   jupyter notebook notebooks/01_pan_genome_pipeline.ipynb
   ```

- Run all cells sequentially. The notebook will:
- Download the 8 genomes automatically.
- Execute sketching -> matching -> visualization.
- Save all figures to the figures/ folder.

---

## Dependencies

The pipeline requires Python 3.9 or higher. All dependencies are versioned in `requirements.txt`.

- `biopython` – genome parsing and BLAST handling
- `numpy`, `scipy`, `pandas` – numerical computing
- `networkx` – flow network construction for min-cost max-flow
- `matplotlib`, `seaborn` – static visualizations
- `logomaker` – sequence logo generation
- `scikit-learn` – dimensionality reduction (t-SNE)
- `requests` – automatic data download

---

## Feature Comparison

| Aspect | Standard Approach | This Pipeline |
| :--- | :--- | :--- |
| Whole-genome distance | Run `sourmash` or `fastANI` | Self-implemented MinHash and LSH |
| Ortholog detection | Use `OrthoFinder` (black-box) | Custom bipartite matching and min-cost flow |
| Pedagogical focus | Tool execution | Algorithm implementation |
| Resume emphasis | Familiarity with existing software | Independent computational problem-solving |

---

## Results Preview

The following figures are generated after a complete pipeline run and exported to the `figures/` folder:

### Figure 1: Phylogenetic Tree
**File:** `figures/phylogenetic_tree.png`  
A distance-based tree illustrating clustering patterns by isolation source.

### Figure 2: Synteny Dot-Plot
**File:** `figures/synteny_dotplot.png`  
A dot-plot comparing K-12 and O157:H7. Off-diagonal clusters indicate large-scale inversions.

### Figure 3: Conservation Logo
**File:** `figures/conservation_logo.png`  
A sequence logo derived from 50 core orthologs, showing extreme conservation in essential functional domains.

---

## Future Extensions

- Integrate linear programming (LP) refinement for abundance correction (applicable to metagenomic contexts).
- Extend the sketching module to support minimizer-based indexing for rapid strain identification.
- Parallelize BLAST computations using Python's `multiprocessing` module to reduce runtime.

---

## References

- Broder, A. Z. (1997). *On the resemblance and containment of documents.* (MinHash foundation).
- Gusfield, D. (1997). *Algorithms on Strings, Trees, and Sequences.* (Background on suffix trees and MSA).
- Edmonds, J., & Karp, R. M. (1972). *Theoretical improvements in algorithmic efficiency for network flow problems.* (Min-cost flow theory).

---

## Contact

- GitHub: [https://github.com/radinbhh](https://github.com/radinbhh)
- LinkedIn: [www.linkedin.com/in/radin-baharsefat-878639231](www.linkedin.com/in/radin-baharsefat-878639231)
