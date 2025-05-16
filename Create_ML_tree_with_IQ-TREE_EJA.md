Creating Maximum Likelihood Tree of all EJA samples with IQ-TREE
================
Jason Toy
2025-04-24


## Convert pruned VCF to aligned sequence file using vcf2phylip v2.8
Download script from https://github.com/edgardomortiz/vcf2phylip, create a new conda environment for it. Then run:
```bash
conda activate /hb/groups/bernardi_lab/programs/vcf2phylip
python ../programs/vcf2phylipdir/vcf2phylip.py -i 8-Embiotoca.pruned.vcf.gz --output-folder aligned_seqs --fasta
```

## Run IQtree on aligned sequence file
```bash
module load iq-tree
iqtree2 -s 8-Embiotoca.pruned.min4.fasta --seqtype DNA -m GTR+ASC+G4 -B 1000 -T AUTO
```
This threw an error saying that 28,590 sites were "constant sites" (invariant). I couldn't quite figure out why they are flagged because all sites in the vcf should be variable, but I suspect it has to do with how IQtree handles ambiguous sites with IUPAC codes like Y or W. Conveniently, IQ tree outputs a new input file you can use that only has the sites it considers variable (72,563 sites in this case)


## Run IQtree on new aligned sequence file
```bash
# model specifed
iqtree2 -s 8-Embiotoca.pruned.min4.fasta.varsites.phy --seqtype DNA -m GTR+ASC+G4 -B 1000 -T 34

# allow IQTree to find best fitting model using ModelFinder Plus
iqtree2 -s 8-Embiotoca.pruned.min4.fasta.varsites.phy --seqtype DNA -m MFP -B 1000 -T 34
```

Best model according to BIC was TVMe+ASC+R5.


## Infer root without an outgroup
```bash
iqtree2 -s 8-Embiotoca.pruned.min4.fasta.varsites.phy --seqtype DNA -m UNREST -B 1000 -T 34 --prefix nonrev_rooted
```
