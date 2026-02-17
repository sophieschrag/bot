# Notebook Log
# data description 
Source: https://datadryad.org/dataset/doi:10.5061/dryad.r7sqv9s97
My data set consists of mitochondrial DNA sequences (cyt b and ND4) from the Caspian Cobra and related Asain Cobras, sampled across Iran, Afghanistan and Turkmenistan for phylogenetic analysis.
# QC
My data does not have raw reads or UCEs
# alignment 
downloaded miniconda and mafft 
# mafft 
MAFFT is a progressive multiple sequence alignment algorithm that uses pairwise distance estimation to construct a guide tree and then aligns sequences progressively according to that tree. It uses Fast Fourier Transform to efficiently detect homologous regions and improve speed compared to traditional dynamic programming methods. I chose this method as it seemed to be the best fit with my data.
assumptions: 
sequences share homologenous regions
insertions and deletions are reasonably modeled with gap penalties 
limitations: 
Progressive alignment errors early in the guide tree can propagate
Performance depends on parameter choice
May struggle with highly divergent sequences
# command used for alignment 
mafft cobra_data > cobra-aligned-mafft.fasta


