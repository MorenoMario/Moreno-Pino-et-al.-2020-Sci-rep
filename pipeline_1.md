## Reads processing
#### Read trimming

```
e.g.
cutadapt -b AGATCGGAAGAGCACACGTCTGAACTCCAGTCA -b AGATCGGAAGAGCGTCGTGTAGGGAAAGAGTGT CGB113-1_rRNA_R1.fastq -q 30 -O 6 -o CGB113-1_rRNA_R1_cutadapt.fastq

```
#### Merge read (R1 and R2)

```
e.g.
pear-0.9.6-bin-64 -v 20 -f CGB113-1_rRNA_R1_cutadapt.fastq -r CGB113-1_rRNA_R2_cutadapt.fastq -o CGB113-1_merge_pear.fastq
```

#### Assembly reads using [Spades-3.6.1](https://cab.spbu.ru/software/spades/)
```
SPAdes-3.6.1-Linux/bin/spades.py --careful --only-assembler --s1 ../CGB113-1_norRNA.fastq -o ./CGB113-1_nonrRNA_spades -t 12 -m 200 &

```
#### Keep contigs >200pb

```
fastalength -f final_contigs.fasta | awk '$1>199{print$2}'  > final_contigs_more_200
seqtk subseq final_contigs.fasta final_contigs_size_more_200.fasta > archivo.fasta

```

#### ORfs prediction
```
MetaGeneMark_linux_64/mgm/gmhmmp -a -d -f G -m mgm/MetaGeneMark_v1.mod -o ./CGB113-1_meta_gen_mark_lst /home/mario/Documentos/final_contigs_size_more_200.fasta -A prot_seq_CGB113-1.gff -D nucl_seq_CGB113-1.gff
```
