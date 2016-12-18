# tapir

tapir is a software for the prediction of plant microRNA targets.

It is described in the paper "TAPIR, a web server for the prediction of plant microRNA targets, including target mimics" by Bonnet et al., published in [Bioinformatics](http://bioinformatics.oxfordjournals.org/content/26/12/1566.abstract).
  


## Installation


     This distribution is for Linux based machines only.

     REQUIRED: The following perl module and libraries must be installed:

     - Bioperl (www.bioperl.org) 
     - ViennaRNA library + perl modules (www.tbi.univie.ac.at/RNA/)

     Unpack the tape archive file in a suitable directory, e.g. /usr/local/

       tar xzvf tapir-1.1.tar.gz

     Include the directory in your PATH

       export PATH="${PATH}:/usr/local/tapir-1.1/"

     You're now ready to go!



## Search with the FASTA engine (fast search)
```
Usage tapir_fasta [options]
    --mir_file mir fasta file
    --target_file target fasta file
    --score <value> score cutoff
    --mfe <value> mfe ratio cutoff

Example:
    tapir_fasta --mir_file 163.fa --target_file AT1G66720.fa 
    tapir_fasta --score 2 --mfe_ratio 0.6 --mir_file 163.fa --target_file AT1G66720.fa
```

## Search with the RNAhybrid engine (precise search)

The search is done in two steps. First running the RNAhybrid program and second parse the raw results. The two steps can be combined by creating a pipe, or done separately. The reason for having two steps is that the RNAhybrid search is quite slow. This step can be parallelized on a cluster, using the first command.

```
RNAhybrid raw results

Usage: tapir_hybrid <mir_file> <target_file>

Parsing the results

Usage hybrid_parser [options]
   --mimic <1> search for target mimics (default 0)
   --score <value> score cutoff (default 4)
   --mfe <value> mfe ratio cutoff (default 0.7)

The option mimic toggles the search for target mimics. The only possible option is to play with the mfe_ratio value.

   Example:
   tapir_hybrid 399a.fa IPS1.fa | hybrid_parser --mimic 1 
   tapir_hybrid 399a.fa IPS1.fa | hybrid_parser --mimic 1 --mfe_ratio 0.6

   If the option mimic is not enabled, then the program is looking for
   microRNA targets, like the tapir_fasta program.

   Example:
   tapir_hybrid 163.fa AT1G66720.fa | hybrid_parser
   tapir_hybrid 163.fa AT1G66720.fa | hybrid_parser --score 2 --mfe_ratio 0.6
```


## Output examples

```
> tapir_fasta --mir_file 163.fa --target_file AT1G66720.fa 

# Search options
# miRNA file 163.fa
# target file AT1G66720.fa
# score <= 3.5
# mfe ratio >= 0.7
#
miRNA         ath-miR163
target        AT1G66720.1
score         2
mfe_ratio     0.91
start         333
seed_gap      0
seed_mismatch 0
seed_gu       1
gap           1
mismatch      0
gu            0
miRNA_3'      UAGCUUCAAGGUUCAGGAGAAGUU
aln           ||||.|||||||o|||||||||||
target_5'     AUCG-AGUUCCAGGUCCUCUUCAA
//


> tapir_hybrid 163.fa AT1G66720.fa | hybrid_parser 

# Search options
# mimic = 0
# score <= 4
# mfe_ratio >= 0.7
#
miRNA          ath-miR163
target         AT1G66720.1
score          2
mfe            -41.8
mfe_ratio      0.8970
start          332
gap            1
mismatch       0
GU             0
seed_gap       0
seed_mismatch  0
seed_GU        1
miRNA_3'       UAGCUUCAAGGUUCAGGAGAAGUU
aln            ||||.|||||||o|||||||||||
target_5'      AUCG-AGUUCCAGGUCCUCUUCAA
//


tapir_hybrid 399a.fa IPS1.fa | hybrid_parser --mimic 1

# Search options
# mimic = 1
# mfe_ratio >= 0.7
#
miRNA     ath-miR399a
target    IPS1-AT3G09922.1
mfe       -31.9
mfe ratio 0.7401
start     237
gap       4
mismatch  2
GU        1
miRNA_3'  GUCCCGUUUAG---AGGAAACCGU
aln       ..||||||.|o...||||||||||
target_5' -UGGGCAACUUCUAUCCUUUGGCA
//
```

## Compilation of the fasta and trh programs

TAPIR is using the FASTA software of Bill Pearson for the fast search option (see http://faculty.virginia.edu/wrpearson/fasta/).

A pre-compiled binary for Linux is provided, but the source code is available in the directory src_fasta/. To compile it, just go in that directory and type the command 'make', after a while and if everything goes well, the binary fasta34 should be available.

TAPIR is using the software RNAhybrid developped by Marc Rehmsmeier for the sensitive search option (see http://bibiserv.techfak.uni-bielefeld.de/rnahybrid/).

I made very small modifications of the source code to get extra output parameters, such as the free energy ratio of the duplex. The modified source code is available in the directory src_trh/. To compile it, just go in that directory and type 'make'. The binary program 'trh' should be ready after the compilation.

## Citation

 If you're using this program, please cite our paper:

  TAPIR, a web server for the prediction of plant microRNA targets, including
  target mimics.
  
  Eric Bonnet, Ying He, Kenny Billiau and Yves Van de Peer
  
  http://bioinformatics.oxfordjournals.org/content/26/12/1566.abstract

## Fee

The usage of this program is free of charge for academics and nonprofit organizations. For any commercial usage, please contact the authors.

