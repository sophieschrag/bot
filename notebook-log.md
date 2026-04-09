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
# Neighbor joining Tree method
- (Saitou & Nei, 1987)
- reconstructs phylogneies from pairwise genetic distances
  **assumptions:** genetic distances accurately reflect evolutionary divergence
  Sites evolve independently according to substitution model
  **advantages:** fast and suitable for larger data sets
  **limitations:** does not explicitely model sequence evolution
# Parsimony-based Phylogeny 
- identifies the tree requiring the fewest evolutionary changes
  **assumptions:** simplest evolutionary tree is most likely
  **advantages:** effective for closely related taxa
  **limitations:** computationally intensive and sensitive to honmoplasy
  
# building phylogenetic trees
- download r-studio
- set my working directory
  setwd("C:/Users/sophi/bot/data")
  **NJ tree method**
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
  **Parsimony method**
  ( same steps different code )
alignment_phydat <- phyDat(as.character(alignment), type = "DNA")
tree_mp <- pratchet(alignment_phydat)

plot(tree_mp, main = "Maximum Parsimony Tree")
write.tree(tree_mp, "mp_tree.nwk")

png("mp_tree.png", width = 1200, height = 1200, res = 150)
plot(tree_mp, main = "Maximum Parsimony Tree")
dev.off()

# Maximum Likelihood Method 
I chose IQ-tree. It builds phylogenetic trees using maximum likelihood. It starts with a bunch of candidate trees (often from maximum parsimony) and keeps updating them as it finds better ones.
**Assumptions:**
1. each site in alignment evolves independently
2. evolution follows substitution model
3. there is a sufficient sequence length
**limitations**
1. computationally intensive for larger sets of dara
2. relies on alignment quality
3. assumes a single-tree explains the data
**ML method steps**
- Run IQ tree on aligned data
(base) PS C:\Users\sophi\Downloads\iqtree-3.1.1-Windows (1)\iqtree-3.1.1-Windows\bin> iqtree3 -s "C:\Users\Sophi\bot\data\cobra-aligned-mafft.fasta"
- check output files (in same folder as input)
# Mr.Bayes 

Convert my aligned data to a .nex file so that it can be run through Mr.Bayes
> alignment <- read.dna("cobra-aligned-mafft.fasta", format = "fasta")
> write.nexus.data(alignment, file = "cobra-aligned-mafft.nex", format = "dna")
**creating my mr.Bayes block**
- No outgroup was specified becayse every sequence in the data set belongs to the genus _Naja._ The phylogeny is inferred as unrooted. Outgroups for future analyses could include closely related elapids.
    [start block]
- begin mrbayes;
    [automatic exit after all commands are run]
    set autoclose=yes 

    [Set priors for branch length, gamma shape parameter, transition/transversion rate ratio/nucleotide base frequencies]
    prset brlenspr=unconstrained:exp(10.0);
    prset shapepr=exp(1.0);
    prset tratiopr=beta(1.0,1.0);
    prset statefreqpr=dirichlet(1.0,1.0,1.0,1.0);

  [Specify the substitution model - default prior for DNA data]
    lset nst=6 rates=invgamma 

    [MCMC settings - parameters for MCMC run]
    mcmcp ngen=10000 samplefreq=100 printfreq=1000 diagnfreq=1000;

    mcmc;
    sumt;
end;


