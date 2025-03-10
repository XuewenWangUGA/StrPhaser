# StrPhaser
A novel bioinformatics software to build STR alleles and phase them using VCF and reference sequence.
* __Building long STR alleles from VCF__

* __Achieve same effects as from long sequencing reads__

*  __Call all variants inside a STR allele at the same time__

* __Run faster with parallel computing__
  
* __Programmed by Ph.D Xuewen Wang for those love genomics and science__


## Computing enviroments 

Works for any computing system where the Java Run environment is available. The tested systems include:

Windows 10,11

Mac OS 11.6.5

Linux: Ubuntu 20.04, 22.04; Center OS 8

## Installation 
The software can be downloaded for a direct use. No additional compiling and installation.  Get it from Github by type the following command in a terminal window: 

`git clone https://github.com/XuewenWangUGA/StrPhaser`

or download the zip compressed files and then unzip to StrPhaser

remove the version number: change the file name of StrPhaserv###.jar , e.g., StrPhaserv1.0.jar to StrPhaser.jar

## Update Java run environment if necessary
The sfotware will use the Java runtime environment (SE) V17. If your computer has an old version of Java runtime, please install the newest Java or Java SE Development Kit 17 or higher from https://www.oracle.com/java/technologies/downloads/. Either Java or Java SE should work.

## The latest version 
v1.1

## Version history

v1.1: add an option -b for the optimized STR construction with user given values, default 3.

v1.0: 1st official release

## License
The tool is under the Lesser General Public License v2.1. Free to distribute and improve. Free for all academic and educational purposes. A license is needed to be obtained from us for any industrial and any other purposes. Please contact us.

## Usage:
Open a terminal window and type the following command: 

`java -jar StrPhaser.jar [options]  >out.tsv`


where "v##" is the version number.

or if want to control the maxi allowed computing memory, add -Xmx option:e.g., the -Xmx4G is for the allowed maximum 4G memory to use,  or changed to any memory you needed, e.g., 2G: -Xmx2G

`java -jar -Xmx4G StrPhaser.jar [options]  >out.tsv`

Options:  
     -b,--base <arg>           integer, the number of offset bases, default [3]
     -r,--referenceSeq <arg>      reference genome sequence file in fasta format, accompanied by an indexed .fai from samtools faidx: eg. hg38.fasta, hg38.fa.fai <br/>
     -s,--strRegion <arg>         STR regions in tabular plain text, 1-based coordinate, one locus per line.  
                                  eg.  Chrom ChromStartPos_Str ChromEndPos_Str Name
                                       chr1 1000 10100 mkName1
                                       chr3 3000 30200 mkName2
    -t,--ThreadNumber <arg>      integer, the number of computing threads, default [1]
    -v,--vcf <arg>               vcf file in .gz and with index gz.tbi from bcftools index --tbi

## Fast speed

The running speed of this tool is very fast. For CODIS core 20 STR sites, it will take less than 1 second with two computing threads for a vcf from 30x 1000 Genome project.

![SpeedImage](speed_time.PNG) Fig.0 Time to analysis CODIS core STRs from one human's VCF from 1000 Genome Project

## Accuracy
The constructed STR alleles have an accuracy up to 100%. Users can adjust the offset value to optimize the accuracy. In addition, the accuracy is effected by the VCF data. If VCF is correctly genotyped, the STR alleles will be accuracte.
       
## Examplar analysis step by step
Here is an example of how to use this tool for a human for 8-kb regions of 20 CODIS core STR sites.  For your specific genome, you just need to replace the genome and targeted sites in configure files with yours.
Firstly go to the software directory in the command window:

` cd StrPhaser`
       
### Step 1. prepare a genome reference file which should be the one used for vcf generation previously 
Download the genome sequence of human from the 1000 Genome Project to the folder "StrPhaser" 
       ftp://ftp.1000genomes.ebi.ac.uk/vol1/ftp/technical/reference/GRCh38_reference_genome/GRCh38_full_analysis_set_plus_decoy_hla.fa	
or use the command: 

`wget ftp://ftp.1000genomes.ebi.ac.uk/vol1/ftp/technical/reference/GRCh38_reference_genome/GRCh38_full_analysis_set_plus_decoy_hla.fa`

Then to index the genome sequence with samtools (tool link: https://www.htslib.org/)  

`samtools faidx GRCh38_full_analysis_set_plus_decoy_hla.fa`
       
       
### Step 2. get test data and targeted site files 
Download the subdirectory `testData` from this page and put it inside the folder "StrPhaser".


### Step 3. run the analysis with the following command
We use the vcf files for human sample HG00130 as the test data. The original files are downloaded from the 1KG project at https://www.internationalgenome.org/data-portal/data-collection/30x-grch38. VCF for all chromosomes are merged and sorted, indexed with bcftools. To use the following command to run :

For Windows system:

`java -jar StrPhaser.jar -r GRCh38_full_analysis_set_plus_decoy_hla.fa -s testData\CODISSTR_anchor.XW.config_v0.3.txt -v testData\CCDG_14151_B01_GRM_WGS_2020-08-05_AllChr.filtered.shapeit2-duohmm-phased.8000.HG00130.vcf.gz -t 2 >out.tsv`

For Linux-like systems:   

`java -jar StrPhaser.jar -r GRCh38_full_analysis_set_plus_decoy_hla.fa -s testData/CODISSTR_anchor.XW.config_v0.3.txt -v testData/CCDG_14151_B01_GRM_WGS_2020-08-05_AllChr.filtered.shapeit2-duohmm-phased.8000.HG00130.vcf.gz -t 2 >out.tsv`


Here the output will be saved to "out.tsv", which is a tab seperated plain text. You can give any file name as like. If no output file name is given, the output will be directed into standout/screen of the terminal window.


## output: 
### results 1. out.tsv

The output will look like:

    #pars: -r GRCh38_full_analysis_set_plus_decoy_hla.fa -s testData/CODISSTR_anchor.XW.config_v0.3.txt -v testData/CCDG_14151_B01_GRM_WGS_2020-08-05_AllChr.filtered.shapeit2-duohmm-phased.8000.HG00130.vcf.gz -t 2 -b 3
    #       time: 2024-02-22 14:40:20.523121603
    #Total loci in STR configure file:      20

    #Marker AlleleID        Length(bp)      Allele(stitched_and_phased)     CallType        Sample
    CSF1PO  a1:     40      ATCTATCTATCTATCTATCTATCTATCTATCTATCTATCT        vcfcall
    CSF1PO  a2:     44      ATCTATCTATCTATCTATCTATCTATCTATCTATCTATCTATCT    vcfcall
    D10S1248        a1:     52      GGAAGGAAGGAAGGAAGGAAGGAAGGAAGGAAGGAAGGAAGGAAGGAAGGAA    vcfcall
    D10S1248        a2:     64      GGAAGGAAGGAAGGAAGGAAGGAAGGAAGGAAGGAAGGAAGGAAGGAAGGAAGGAAGGAAGGAA        vcfcall
    D12S391 a1:     76      AGATAGATAGATAGATAGATAGATAGATAGATAGATAGATAGATAGACAGACAGACAGACAGACAGACAGACAGAT    vcfcall
    D12S391 a2:     76      AGATAGATAGATAGATAGATAGATAGATAGATAGATAGATAGATAGATAGACAGACAGACAGACAGACAGACAGAT    vcfcall
    D13S317 a1:     44      TATCTATCTATCTATCTATCTATCTATCTATCTATCTATCTATC    vcfcall
    D16S539 a1:     48      GATAGATAGATAGATAGATAGATAGATAGATAGATAGATAGATAGATA        vcfcall
    D16S539 a2:     44      GATAGATAGATAGATAGATAGATAGATAGATAGATAGATAGATA    vcfcall
    D18S51  a1:     60      AGAAAGAAAGAAAGAAAGAAAGAAAGAAAGAAAGAAAGAAAGAAAGAAAGAAAGAAAGAA    vcfcall
    D18S51  a2:     52      AGAAAGAAAGAAAGAAAGAAAGAAAGAAAGAAAGAAAGAAAGAAAGAAAGAA    vcfcall
    D19S433 a1:     68      CCTTCCTTCCTTCCTTCCTTCCTTCCTTCCTTCCTTCCTTCCTTCCTTCCTTCCTACCTTCTTTCCTT    vcfcall
    D1S1656 a1:     68      CCTATCTATCTATCTATCTATCTATCTATCTATCTATCTATCTATCTATCTATCTATCTATCTATCTA    vcfcall
    D21S11  a1:     127     TCTATCTATCTATCTATCTGTCTGTCTGTCTGTCTGTCTGTCTATCTATCTATATCTATCTATCTATCATCTATCTATCCATATCTATCTATCTATCTATCTATCTATCTATCTATCTATCTATCTA   vcfcall
    D21S11  a2:     123     TCTATCTATCTATCTATCTGTCTGTCTGTCTGTCTGTCTGTCTATCTATCTATATCTATCTATCTATCATCTATCTATCCATATCTATCTATCTATCTATCTATCTATCTATCTATCTATCTA       vcfcall
    D22S1045        a1:     45      ATTATTATTATTATTATTATTATTATTATTATTATTACTATTATT   vcfcall
    D22S1045        a2:     33      ATTATTATTATTATTATTATTATTACTATTATT       vcfcall
    D2S1338 a1:     96      GGAAGGAAGGACGGAAGGAAGGAAGGAAGGAAGGAAGGAAGGAAGGAAGGAAGGAAGGAAGGAAGGAAGGCAGGCAGGCAGGCAGGCAGGCAGGCA        vcfcall
    D2S1338 a2:     92      GGAAGGAAGGACGGAAGGAAGGAAGGAAGGAAGGAAGGAAGGAAGGAAGGAAGGAAGGAAGGAAGGCAGGCAGGCAGGCAGGCAGGCAGGCA    vcfcall
    D2S441  a1:     40      TCTATCTATCTATCTATCTATCTATCTATCTATCTATCTA        vcfcall
    D2S441  a2:     56      TCTATCTATCTATCTATCTATCTATCTATCTATCTATCTATCTATTTATCTATCTA        vcfcall
    D3S1358 a1:     64      TCTATCTGTCTGTCTATCTATCTATCTATCTATCTATCTATCTATCTATCTATCTATCTATCTA        vcfcall
    D5S818  a1:     44      ATCTATCTATCTATCTATCTATCTATCTATCTATCTATCTATCT    vcfcall
    D5S818  a2:     52      ATCTATCTATCTATCTATCTATCTATCTATCTATCTATCTATCTATCTATCT    vcfcall
    D7S820  a1:     40      TATCTATCTATCTATCTATCTATCTATCTATCTATCTATC        vcfcall
    D7S820  a2:     32      TATCTATCTATCTATCTATCTATCTATCTATC        vcfcall
    D8S1179 a1:     40      TCTATCTATCTATCTATCTATCTATCTATCTATCTATCTA        vcfcall
    D8S1179 a2:     48      TCTATCTATCTATCTATCTATCTATCTATCTATCTATCTATCTATCTA        vcfcall
    FGA     a1:     88      GGAAGGAAGGAGAAAGAAAGAAAGAAAGAAAGAAAGAAAGAAAGAAAGAAAGAAAGAAAGAAAGAAAGAGAAAAAAGAAAGAAAGAAA        vcfcall
    TH01    a1:     28      AATGAATGAATGAATGAATGAATGAATG    vcfcall
    TH01    a2:     20      AATGAATGAATGAATGAATG    vcfcall
    TPOX    a1:     32      AATGAATGAATGAATGAATGAATGAATGAATG        vcfcall
    vWA     a1:     60      TAGATAGATAGATAGATAGATAGATAGATAGATAGATAGACAGACAGACAGACAGATAGA    vcfcall
    vWA     a2:     64      TAGATAGATAGATAGATAGATAGATAGATAGATAGATAGATAGACAGACAGACAGACAGATAGA        vcfcall
    #       time: 2024-02-22 14:40:20.973171661
    #Warning: colorful DNA display is not supported

    

The # section is the comments.

In the output file, there will have many columns. for a diploid genome, there will be at least one allele. For polyploidy genome, there may have have more then one allele for each marker/locus. 

The "Marker" column contains the same name a locus or marker as given in configure file.

The "AlleleID" column is the name of phased alleles. a0 for a reference call, a1, and a2 for alternatice alleles different from reference genome sequence.

The "Length(bp)" column is the bp of bases.

The "Allele(stitched_and_phased)" is the allelic sequences of a STR.

The "CallType" has the information suggesting from reference seq (RefCall) or vcf (vcfcall).

The "Sample" may have the information of the sample name inherited from vcf.

the file can be opened in Microsoft Excel or other spreadsheet for easy read. For all 20 CODIS sites, please download the out.tsv in the directory "testData_output" in this Github site.

### results 2. display colorful alleles .html

Another result called output .html will be also generated for a colorful display of the alleles if supported by your system. It should be supported in most computing systems but not all, like outdated CentOS with the command support only. This file will be opened in an internet web browser by default if available.

![ColorAlleleImage](D8_colorAllele.PNG) Fig.1 Colorful alleles of locus D8 output 

![ColorAlleleImage](D19_colorAllele.PNG) Fig.2 Colorful alleles of locus D19 output 



## Citation
Xuewen Wang, Jonathan King, Huang Meng, Michael D. Coble,  August E. Woerner. 2025, StrPhaser constructs tandem repeat alleles from VCF data.
doi: https://doi.org/10.1101/2025.01.22.634325

## Funding
This work was sponsored in part by award 15PNIJ-21-GG-04159-RESS, awarded by the National Institute of Justice, Office of Justice Programs, U.S. Department of Justice.

## Questions

Please post or contact me if you have any questions.

       


                              
