Day 3 Homework Solution
------------------------

    cat region.bed | while read line; do START=`echo -n $line | cut -d' ' -f2`; END=`echo -n $line | cut -d' ' -f3`; LEN=`expr $END - $START`; echo $LEN; done

    #!/bin/bash
    cat region.bed | grep ^$1 | while read line; do START=`echo -n $line | cut -d' ' -f2`; END=`echo -n $line | cut -d' ' -f3`; LEN=`expr $END - $START`; echo $LEN; done
