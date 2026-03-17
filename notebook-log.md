# Notebook Log
# data description 
Source: https://datadryad.org/dataset/doi:10.5061/dryad.r7sqv9s97
My data set consists of mitochondrial DNA sequences (cyt b and ND4) from the Caspian Cobra and related Asain Cobras, sampled across Iran, Afghanistan and Turkmenistan for phylogenetic analysis.
# QC
My data does not have raw reads or UCEs
# alignment 
downloaded miniconda and mafft 
**Alignment method chosen:** MAFFT
**Algorithm:** Progressive alignment using FFT. Primarily homologous
**Assumptions:** Input sequences are homologous
**Limitations:** May produce less accurate alignments for highly divergent sequences
# command used for alignment 
mafft cobra_data > cobra-aligned-mafft.fasta
# Maximum Likelihood Method 
I chose IQ-tree - estimates aphylogenetic tree that makes the observed sequence data most probable
# Assumptions: 
1. each site in alignment evolves independently
2. evolution follows substitution model
3. tree is bifuricating
# limitations
1. computationally intensive for large datasets
2. results depend on model selection
3. may get stuck in local optima instead of the true global best tree
**# ML method steps 
**# Step 1: Run IQ-TREE

iqtree2 -s my_alignment.fasta -m MFP -bb 1000 -nt AUTO

 - '-s my_alignment.fasta': Input alignment file
 - '-m MFP': Automatically selects the best substitution model
 - '-bb 1000': Perform 1000 ultrafast bootstrap replicates for branch support
 - '-nt AUTO': Use all available CPU threads
#- This script is fully reproducible: running it again will perform the same analysis******
**

