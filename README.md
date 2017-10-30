# overlapgenie
Perl software for estimating dN/dS in overlapping genes using the Wei-Zhang method.

## Background
Given the codon triplet nature of the genetic code and the two antiparallel strands of the DNA molecule, a single segment of DNA has the potential to encode 6 reading frames: three in the forward (sense) direction and three in the reverse (antisense) direction. Although all possible frames are seldom if ever used, a substantial fraction of genes in taxa ranging from viruses to humans encode overlapping gene (OLG) pairs, usually running in opposite directions (sense-antisense, or sas) (Sabath 2009). 

Unfortunately, standard measures of natural selection such as dN/dS do not apply to OLGs, because a mutation that is synonymous in one frame may be nonsynonymous in another, and vice versa. Thus, it is necessary to develop improved measures or OLGs. Based on the method and scripts of Wei and Zhang (2014), **overlapgenie** is an in-development Perl pipeline designed to automate the process of calculating dN/dS for OLGs. In short, the Wei-Zhang method is a counting method analogous to the Nei-Gojobori (1986) method, but for use with OLGs. It considers the effects of mutations in the overlapping frame to determine the numerator and denominator of dN and dS.  For example, dN is usually calculated as the mean number of nonsynonymous (amino acid changing) nucleotide differences per nonsynonymous nucleotide site, and dS is similarly calculated for synonymous (silent) differences and sites. In order to control for the possibility that synonymous sites in the frame of interest may be under selection in the alternative overlapping reading frame, we can instead consider the expanded measures dNN, dNS, dSN, and dSS, where the first subscript refers to the reference (sense) gene, and the second to the alternative (sense or antisense) OLG. For example, dNN refers to the normalized number of differences which are nonsynonymous in both the reference and the alternative frames (i.e., NN). Using these measures, it is possible to calculate dN/dS for the reference gene as dNN/dSN or dNS/dSS, and to calculate the same for the alternative overlapping gene as dNN/dNS or dSN/dSS, i.e., the subscript in the alternative OLG is held constant to control for OLG effects. 

## Method
As of now, **overlapgenie** has only been implemented for one OLG relationship: sas12, in which sense-antisense genes overlap such that the reverse strand second codon positions correspond to codon position 1 in the sense reference gene. In other words, the sense gene's first codon position overlaps the antisense gene's second codon position: 

<img src="https://github.com/chasewnelson/overlapgenie/blob/master/sas12_figure.png" alt="sas12 logo" align="middle">

This is based on the subroutines of Wei and Zhang's (2014) **sas12_data.pl** script, which performed a dN/dS calculation for a single sequence pair with no gaps. Overlapgenie expands upon this by allowing command-line user input for an arbitrary number of sequences (with (n^2-n)/2 pairwise comparisons) which may contain gaps.

## How to Use
**Overlapgenie** accepts as input three necessary parameters: (1) a single aligned FASTA file; (2) a transition/transversion ratio (R); and (3) a number of parallel processes to be used on a computer cluster. It is necessary for Perl's Parallel::ForkManager must be installed, allowing one to take advantage of argument (3). A typical modern laptop might use 4 processes (overlapgenie's default value); a high-performance cluster can use many more (e.g., 100).

Call **overlapgenie** as follows:

        perl <overlapgenie_script>.pl <reference_gene>.fasta <R> <ncpus>

where <overlapgenie_script> script refers to the OLG relationship of interest (e.g., overlapgenie_sas12); the <referenceGene> refers to a FASTA file of the reference gene for all individuals or taxa; <R> is the transition/transversion ratio; and <ncpus> is the number of parallel processes to be invoked. For example:

        perl overlapgenie_sas12.pl reference_OLG_homologs.fasta 2.2 4

Here, **overlapgenie_sas12.pl** refers to the script used for the OLG sas(sense-antisense)12 relationship (see above). myReferenceGene.fasta is an aligned FASTA file containing the genes to compare. The value 2.2 is the independently-determined transition/transversion ratio. Finally, we will use 4 parallel processes to speed up the calculation.

## COMING SOON
New OLG relationships are on the way in the future. Three other situations require completion: sas11, sas13, and ss(sense-sense)12, the latter of which is equivalent to ss13 if the reference and alternative OLGs are reversed. Stay tuned.

## References
* Nei M, Gojobori T. 1986. Simple methods for estimating the numbers of synonymous and nonsynonymous nucleotide substitutions. Mol. Biol. Evol. 3:418–426.
* Sabath N. 2009. Molecular Evolution of Overlapping Genes. ProQuest Dissertation #3405062.* Sabath N, Landan G, Graur D. 2008. A method for the simultaneous estimation of selection intensities in overlapping genes. PLoS One 3(12): e3996.* Wei X, Zhang J. 2014. A simple method for estimating the strength of natural selection on overlapping genes. Genome Biol. Evol. 7:381–390.