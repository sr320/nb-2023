# ATAC
2023-06-13

# Background

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

``` bash
ls ../data/atac-plus/ | sort
```

    20200526_Cgigas_epi_compare.xml
    5GRO-DB107-180430.1nt.neg.bw
    5GRO-DB107-180430.1nt.pos.bw
    5GRO-comb.1nt.neg.bw
    5GRO-comb.1nt.pos.bw
    5GRO.tss.txt.bed
    ATAC-JHS696-170217.cov.bw
    ATAC-JHS697-170223.cov.bw
    ATAC-JHS698-170217.cov.bw
    ATAC-JHS699-170223.cov.bw
    ATAC-comb.cov.bw
    ATAC.cov.bw
    ATAC.peaks.bed
    ATAC.peaks.txt.bed
    RNA.neg.bw
    RNA.pos.bw
    RNA.transcripts.gtf
    RNA.transcripts.gtf.idx
    csRNA-JHS670-170224.1nt.neg.bw
    csRNA-JHS670-170224.1nt.pos.bw
    csRNA-JHS671-170228.1nt.neg.bw
    csRNA-JHS671-170228.1nt.pos.bw
    csRNA-JHS817-170322.1nt.neg.bw
    csRNA-JHS817-170322.1nt.pos.bw
    csRNA-JHS818-170322.1nt.neg.bw
    csRNA-JHS818-170322.1nt.pos.bw
    csRNA-comb.1nt.neg.bw
    csRNA-comb.1nt.pos.bw
    csRNA.tss.txt.bed
    csRNAinput-JHS718-170228.1nt.neg.bw
    csRNAinput-JHS718-170228.1nt.pos.bw
    csRNAinput-JHS719-170228.1nt.neg.bw
    csRNAinput-JHS719-170228.1nt.pos.bw
    csRNAinput-JHS796-170323.1nt.neg.bw
    csRNAinput-JHS796-170323.1nt.pos.bw
    csRNAinput-JHS797-170323.1nt.neg.bw
    csRNAinput-JHS797-170323.1nt.pos.bw
    csRNAinput-comb.1nt.neg.bw
    csRNAinput-comb.1nt.pos.bw
    input.tss.bed
    mSTART.1nt.neg.bw
    mSTART.1nt.pos.bw
    mSTART.tss.bed
    mSTARTinput.1nt.neg.bw
    mSTARTinput.1nt.pos.bw
    readme.txt

# There is an IGV session file…

![igv](http://gannet.fish.washington.edu/seashell/snaps/Monosnap_IGV_-_Session_Userssr320DocumentsGitHubnb-2023Cgigasdataatac-plus20200526_Cgigas_epi_compare.xml_2023-06-13_21-27-28.png)

The types of these files can be inferred based on their extensions:

1.  `.bw`: These are BigWig files, a binary file format used to store
    genomic data (like scores for each feature) that can be densely
    packed into an interval tree for rapid access. BigWig files are
    often used for read coverage of sequence alignment files, among
    other things.

2.  `.bed`: BED files are a file format used to store genomic regions as
    coordinates and associated annotations. The format is often used in
    genome-sequencing projects.

3.  `.txt`: This is a standard text file that contains human-readable
    characters. Text files can be opened by a variety of text editors
    and can contain a wide range of information.

4.  `.xml`: XML files are a type of structured document format that is
    used to store data. It is similar to HTML but does not have
    predefined tags. Instead, the user defines the tags to create a
    document structure that suits their needs.

5.  `.gtf`: This is a GTF (Gene Transfer Format) file, which is a
    refinement of GFF (General Feature Format). GTF files are used to
    hold information about gene structure and are often used in
    bioinformatics for genome annotation.

6.  `.gtf.idx`: This is likely an index file related to the GTF file,
    used to make access to the data within the GTF file faster.

7.  `readme.txt`: This is typically a plain text file containing
    information about other files in a directory or archive, often used
    to provide documentation for a project or software.

Most of these files are commonly used in the field of genomics and
bioinformatics. For instance, the BigWig and BED files are commonly used
to visualize genomic data in genome browsers. The GTF files are used to
hold information about gene structures.

------------------------------------------------------------------------

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

# 5GRO

5’GRO stands for 5’ Global Run-On sequencing. This is a technique used
in genomics and transcriptomics to map the 5’ ends of nascent RNAs.

The technique is used to identify the position and orientation of
transcriptionally engaged RNA polymerases genome-wide, and to detect
primary transcription initiation sites. It is often used in studies of
transcription regulation, and can provide insight into the transcription
dynamics of a given cell or tissue type.

The method is similar to Global Run-On Sequencing (GRO-seq), but with a
focus on the 5’ ends of transcripts. By sequencing the 5’ ends of RNAs,
5’GRO-seq can provide a high-resolution map of transcription start sites
across the genome.

# csRNA-seq

Capped small RNA-seq (csRNA-seq) accurately maps RNA polymerase II
transcription start sites (TSSs) from any tissue or eukaryotic organism
where RNA can be extracted. csRNA-seq is an adaptation of Start-seq
(Nechaev et al., Scruggs et al.) and small RNA sequencing (Lister et
al., Gu et al.). The method captures small (\<60 nt), 5’-capped RNAs
which associate with newly initiated RNAs, providing data similar to
Start-seq or 5’GROseq/GROcap(Kruesi et al., Lam et al.). However,
csRNA-seq works with total RNA, eliminating the requirement of nuclei
isolation or run-on. csRNA-seq is thus widely applicable and facilitates
accurate maps of TSSs of stable (i.e. mRNAs, ncRNAs) and unstable
transcripts (i.e. enhancer RNAs, pre-miRNAs, divergent transcripts,
antisense transcripts) from fresh, frozen or archived tissues or
samples - basically any material where quality RNA can be isolated from.

``` bash
ls ../data/atac-plus/cs*
```

    ../data/atac-plus/csRNA-JHS670-170224.1nt.neg.bw
    ../data/atac-plus/csRNA-JHS670-170224.1nt.pos.bw
    ../data/atac-plus/csRNA-JHS671-170228.1nt.neg.bw
    ../data/atac-plus/csRNA-JHS671-170228.1nt.pos.bw
    ../data/atac-plus/csRNA-JHS817-170322.1nt.neg.bw
    ../data/atac-plus/csRNA-JHS817-170322.1nt.pos.bw
    ../data/atac-plus/csRNA-JHS818-170322.1nt.neg.bw
    ../data/atac-plus/csRNA-JHS818-170322.1nt.pos.bw
    ../data/atac-plus/csRNA-comb.1nt.neg.bw
    ../data/atac-plus/csRNA-comb.1nt.pos.bw
    ../data/atac-plus/csRNA.tss.txt.bed
    ../data/atac-plus/csRNAinput-JHS718-170228.1nt.neg.bw
    ../data/atac-plus/csRNAinput-JHS718-170228.1nt.pos.bw
    ../data/atac-plus/csRNAinput-JHS719-170228.1nt.neg.bw
    ../data/atac-plus/csRNAinput-JHS719-170228.1nt.pos.bw
    ../data/atac-plus/csRNAinput-JHS796-170323.1nt.neg.bw
    ../data/atac-plus/csRNAinput-JHS796-170323.1nt.pos.bw
    ../data/atac-plus/csRNAinput-JHS797-170323.1nt.neg.bw
    ../data/atac-plus/csRNAinput-JHS797-170323.1nt.pos.bw
    ../data/atac-plus/csRNAinput-comb.1nt.neg.bw
    ../data/atac-plus/csRNAinput-comb.1nt.pos.bw

# ATAC

``` bash
ls ../data/atac-plus/ATAC*
```

    ../data/atac-plus/ATAC-JHS696-170217.cov.bw
    ../data/atac-plus/ATAC-JHS697-170223.cov.bw
    ../data/atac-plus/ATAC-JHS698-170217.cov.bw
    ../data/atac-plus/ATAC-JHS699-170223.cov.bw
    ../data/atac-plus/ATAC-comb.cov.bw
    ../data/atac-plus/ATAC.cov.bw
    ../data/atac-plus/ATAC.peaks.bed
    ../data/atac-plus/ATAC.peaks.txt.bed

``` bash
tail ../data/atac-plus/ATAC*bed
```

    ==> ../data/atac-plus/ATAC.peaks.bed <==
    scaffold962 687697  687870  scaffold962-461 1   +
    scaffold980 288225  288398  scaffold980-224 1   +
    scaffold1173    29013   29186   scaffold1173-19 1   +
    scaffold1578    16001   16174   scaffold1578-50 1   +
    scaffold1753    106992  107165  scaffold1753-39 1   +
    scaffold180 35570   35743   scaffold180-18  1   +
    scaffold1842    63318   63491   scaffold1842-24 1   +
    scaffold62  863 1036    scaffold62-79   1   +
    scaffold977 382155  382328  scaffold977-115 1   +
    scaffold22  693214  693387  scaffold22-684  1   +

    ==> ../data/atac-plus/ATAC.peaks.txt.bed <==
    scaffold32  175640  175841  scaffold32-21   1   +   175640  175841  0,0,255
    scaffold37900   25359   25560   scaffold37900-3 1   +   25359   25560   0,0,255
    scaffold588 390042  390243  scaffold588-80  1   +   390042  390243  0,0,255
    scaffold632 230369  230570  scaffold632-79  1   +   230369  230570  0,0,255
    scaffold976 46879   47080   scaffold976-19  1   +   46879   47080   0,0,255
    C26156  2808    3009    C26156-2    1   +   2808    3009    0,0,255
    scaffold395 317495  317696  scaffold395-57  1   +   317495  317696  0,0,255
    scaffold41806   2244    2445    scaffold41806-29    1   +   2244    2445    0,0,255
    scaffold610 556440  556641  scaffold610-95  1   +   556440  556641  0,0,255
    scaffold65  104601  104802  scaffold65-5    1   +   104601  104802  0,0,255

``` bash
```

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
