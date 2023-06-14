# ATAC

## Background

Data here:
https://gannet.fish.washington.edu/metacarcinus/Cgigas/shelly/

First step is probably ensuring that there are BEDfiles for each data
type (ATAC-Seq, csRNA-seq, 5’GRO, mSTART). Next step is figuring out
what the data tells us.

``` bash
wget -r \
--no-directories --no-parent \
-P ../data/atac-plus \
https://gannet.fish.washington.edu/metacarcinus/Cgigas/shelly/
```

Goal is to intersectBED these files with DML lists and understand how
methylation is impacted by TSS accessibility, etc.

Additional information from Sascha:

Step by step: 5’GRO similar to csRNA-seq gives you the TSS. for
csRNA-seq, you need an input library.
http://homer.ucsd.edu/homer/ngs/csRNAseq/index.html is the tutorial

``` bash
ls ../data/atac-plus/5*
```

    ../data/atac-plus/5GRO-DB107-180430.1nt.neg.bw
    ../data/atac-plus/5GRO-DB107-180430.1nt.pos.bw
    ../data/atac-plus/5GRO-comb.1nt.neg.bw
    ../data/atac-plus/5GRO-comb.1nt.pos.bw
    ../data/atac-plus/5GRO.tss.txt.bed

here a quick warning: the data passed QC. But was processed in bulk with
50 species. A careful evaluation from an oyster expert may be getting
even better TSS calls etc out of the data as Chris Benner (who did this
analysis) used a script to set thresholds etc.

You find data for two individuals. for which i differentially took
tissue from the branchies and the “muscle/stick” aka Coeur. in the
csRNA-seq data, Oyster_8 and Oyster_9 is the Coeur. Again, my apologies
for the sloppy labeling but I did about 50 species back then and we went
through different “automated” systems… aka codes make sense to us but
noone else.

as for the RNA-seq data, sorry they are badly labeled. The JHS is the
identifier \# but if you want to match the RNA-seq with the csRNA-seq
(i.e. to call stable or unstable TSS):

JHS 741 Oyster1_Branchia_6_Ribo0_Core JHS 742
Oyster2_Branchia_7_Ribo0_Core JHS 743 Oyster1_Coeur_8\_\_Ribo0_Core JHS
744 Oyster2_Coeur_9\_\_Ribo0_Core
