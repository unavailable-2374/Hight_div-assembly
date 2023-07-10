# Zhou Lab @AGIS High-Div-Assembly-Pipeline
This is a workflow that assembles high-diversity regions.

Assembly 

    hifiasm -o grape22.asm -t 32 PN_40024.fq.gz

Subseq the high diversity region

    seqtk subseq PN40024_T2T.fa hight_div.bed > hight_div.fa

Get the haploid region

    for i in {1..2}
    do
        ragtag.py correct PN40024_T2T.fa grape2.asm.bp.hap${i}.p_ctg.fa -t 4 -o PN_hap${i}_cor
        ragtag.py scaffold N40024_T2T.fa grape2.asm.bp.hap${i}.p_ctg.fa -C -t 4 -o PN_hap${i}_sca
        nucmer --mum -t 4 hight_div.fa PN_hap${i}_sca/ragtag.scaffold.fasta -p hap${i}
        delta-filter -1 hap${i}.delta > hap${i}.best.filter
        show-coords -T -q -H hap${i}.best.filter > hap${i}.coord
    done

Annotation

    liftoff -g PN40024.gff3 -o hight_div_hap1.gff3 hight_div_hap1.fa PN40024_T2T.fa -polish -copies -p 5
