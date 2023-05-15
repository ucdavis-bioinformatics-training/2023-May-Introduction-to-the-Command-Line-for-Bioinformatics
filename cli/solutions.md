Challenge & Homework Solutions
==============================

Day 2 homework solution
-----------------------

    wget http://igenomes.illumina.com.s3-website-us-east-1.amazonaws.com/Escherichia_coli_K_12_MG1655/NCBI/2001-10-15/Escherichia_coli_K_12_MG1655_NCBI_2001-10-15.tar.gz
    tar -xvzf Escherichia_coli_K_12_MG1655_NCBI_2001-10-15.tar.gz
    find Escherichia_coli_K_12_MG1655 -name 'refGene.txt'
    find Escherichia_coli_K_12_MG1655 -name 'refGene.txt' -exec ls -l {} \;
    find Escherichia_coli_K_12_MG1655 -name 'adapter_contam1.fa'
    tail -1 Escherichia_coli_K_12_MG1655/NCBI/2001-10-15/Sequence/AbundantSequences/adapter_contam1.fa | tr -d "\n" | wc -c

Day 3 challenge solutions
-------------------------

    sed -i 's/CHR//gi' region.bed
    zcat C61.subset.fq.gz | sed -n '2~4p' | cut -c1-5 | sort | uniq -c

Quiz 5 answers commands
-----------------------

    grep -o ".........TGA" phix.fa | cut -c1-3 | sort | uniq -c | sort -rn
    cat genome.fa | tail -1 | cut -c1-100 | grep -o . | grep -c A

Day 3 Homework solution
-----------------------

    cat region.bed | while read line; do START=`echo -n "$line" | cut -f2`; END=`echo -n "$line" | cut -f3`; LEN=`expr $END - $START`; echo $LEN; done

    #!/bin/bash
    cat region.bed | grep -w ^$1 | while read line; do START=`echo -n "$line" | cut -f2`; END=`echo -n "$line" | cut -f3`; LEN=`expr $END - $START`; echo $LEN; done
