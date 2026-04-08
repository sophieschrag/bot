# Notebook Log
 
# project structure 
project is organized as follows: 
- 'data/' -> raw and aligned sequence data
- 'scripts/' -> scripts
- 'results/' -> output files
- 'figures/'
- 'manuscripts/'

# Workflow Overview 
1. obtain sequence data
2. perform sequence alignment using MAFFT
3. Infer phylogenetic tree using maximum likelihood
4. evaluate

   - changes staged using 'git add'
   - saved using 'git commit'
   - uploaded using 'git push'

  **Alignment method chosen:** MAFFT
**Algorithm:** Progressive alignment using FFT. Primarily homologous
**Assumptions:** Input sequences are homologous
**Limitations:** May produce less accurate alignments for highly divergent sequences
# Alignment software Download
- download Ubuntu and mafft from their official sites.
- extract MAFFt zip to a folder
- Open Ubuntu terminal  

# data description 
Source: https://datadryad.org/dataset/doi:10.5061/dryad.r7sqv9s97
My data set consists of mitochondrial DNA sequences (cyt b and ND4) from the Caspian Cobra and related Asain Cobras, sampled across Iran, Afghanistan and Turkmenistan for phylogenetic analysis.
# QC
My data does not have raw reads or UCEs
# command used for alignment 
- run data through mafft and retitle 
mafft cobra_data > cobra-aligned-mafft.fasta (on WSL terminal)
- check to ensure mafft worked by showing first 10 lines
  head cobra-aligned-mafft.fasta

# building phylogenetic trees
- download r-studio
- set my working directory
  setwd("C:/Users/sophi/bot/data")
- load the alignment (neighbor-joining tree method)
  alignment <- read.dna("cobra-aligned-mafft.fasta", format = "fasta")
- compute the genetic distances
  dist_matrix <- dist.dna(alignment, model = "K80")
- construct the neighbor-joining tree
  tree_nj <- nj(dist_matrix)
- save the tree as an image to include in my repository
  png("nj_tree.png", width = 1200, height = 1200, res = 150)
plot(tree_nj, main = "Neighbor-Joining Tree of Asian Cobras")
dev.off()
- Parsimony method ( same steps different code )
alignment_phydat <- phyDat(as.character(alignment), type = "DNA")
tree_mp <- pratchet(alignment_phydat)

plot(tree_mp, main = "Maximum Parsimony Tree")
write.tree(tree_mp, "mp_tree.nwk")

png("mp_tree.png", width = 1200, height = 1200, res = 150)
plot(tree_mp, main = "Maximum Parsimony Tree")
dev.off()
# Maximum Likelihood Method 
I chose IQ-tree - estimates aphylogenetic tree that makes the observed sequence data most probable


**Assumptions:**
1. each site in alignment evolves independently
2. evolution follows substitution model
3. tree is bifuricating

   
**limitations**
1. computationally intensive for large datasets
2. results depend on model selection
3. may get stuck in local optima instead of the true global best tree

   
**ML method steps** 
**# Step 1: Run IQ-TREE

(base) PS C:\Users\sophi\bot\iqtree\iqtree-3.1.0-Windows\bin> .\iqtree3.exe -s C:\Users\sophi\bot\cobra-aligned-mafft.fasta -m MFP -bb 1000 -nt AUTO

 - '-s cobra-aligned-mafft.fasta': Input alignment file
 - '-m MFP': Automatically selects the best substitution model
 - '-bb 1000': Perform 1000 ultrafast bootstrap replicates for branch support
 - '-nt AUTO': Use all available CPU threads
#- This script is fully reproducible: running it again will perform the same analysis******
**

