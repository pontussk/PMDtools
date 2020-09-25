# PMDtools
Compute postmortem damage patterns and decontaminate ancient genomes. 

[![install with bioconda](https://img.shields.io/badge/install%20with-bioconda-brightgreen.svg?style=flat-square)](http://bioconda.github.io/recipes/pmdtools/README.html)


## Description

PMDtools computes ancient DNA damage patterns, and implements a likelihood framework incorporating postmortem damage (PMD), base quality scores and biological polymorphism to identify degraded DNA sequences that are unlikely to originate from modern contamination. Using the model, each sequence is assigned a PMD score, for which positive values indicate support for the sequence being genuinely ancient. For details of the method, please see the main paper in PNAS.

PMDtools takes SAM-formatted input, and requires an MD tag with alignment information. The MD tag is featured in the output of many aligners but can otherwise be added e.g. using the SAMtools fillmd/calmd tool (Li, Handsaker et al. 2009).

No external packages except for python 2.6+/python3 are required, but for manipulating BAM files, SAMtools is recommended. Extra plotting requires R.

Questions can be addressed to pontus.skoglund@gmail.com.

## Basic usage

### Plotting ancient DNA damage patterns
To compute deamination-derived damage patterns separating CpG and non-CpG sites, enter:

```
samtools view mybam.bam | python pmdtools.0.60.py --platypus --requirebaseq 30 > PMD_temp.txt

R CMD BATCH plotPMD.v2.R

cp PMD_plot.frag.pdf PMD.plot.MYBAM.pdf
```

This allows computing damage patterns from sequence libraries of mammalian nuclear DNA in which damage has been repaired, e.g. using uracil–DNA–glycosylase and endonuclease VIII. This is done by restricting to nucleotides in a CpG context, for which deamination of methylated Cytosine results in Thymine.

### Damage patterns in a single number with statistical uncertainty
A useful tool when screening many libraries is to compute deamination-derived damage patterns at the 5' position with a bionomial standard error:

```
samtools view mybam.bam | python pmdtools.0.60.py --first --requirebaseq 30
```
This estimate with a standard error only for the first position can also be obtained on UDG/USER-treated data for mammalian nuclear DNA by restricting to CpG sites:
```
samtools view mybam.bam | python pmdtools.0.60.py --first --requirebaseq 30 --CpG
```

### Separating ancient DNA molecules from others
To use a likelihood framework restrict to sequences with a PMD score of at least 3, enter:
```
samtools view -h mybam.bam | python pmdtools.0.60.py --threshold 3 --header | samtools view -Sb - > mybam.pmds3filter.bam
```


For a full list of options, enter
```
python pmdtools.py --help
```

### Citation
Please cite: P Skoglund, BH Northoff, MV Shunkov, A Derevianko, S Pääbo, J Krause, M Jakobsson (2014) *Separating ancient DNA from modern contamination in a Siberian Neandertal*, Proceedings of the National Academy of Sciences USA doi:10.1073/pnas.1318934111

 ![](https://github.com/pontussk/PMDtools/blob/master/PMD_Skoglund_et_al_2015_Current_Biology.png?raw=true)

