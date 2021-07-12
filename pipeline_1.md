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
#### Sort rRNA and nonRNA reads

```
nohup ../sortmerna --ref /media/MD3200_VD1/Software/sortmerna-2.0-linux-64/rRNA_databases/silva-euk-28s-id98.fasta,/media/MD3200_VD1/Software/sortmerna-2.0-linux-64/index/silva-euk-28s:/media/MD3200_VD1/Software/sortmerna-2.0-linux-64/rRNA_databases/silva-euk-18s-id95.fasta,/media/MD3200_VD1/Software/sortmerna-2.0-linux-64/index/silva-euk-18s-db:/media/MD3200_VD1/Software/sortmerna-2.0-linux-64/rRNA_databases/silva-bac-23s-id98.fasta,/media/MD3200_VD1/Software/sortmerna-2.0-linux-64/index/silva-bac-23s-db:/media/MD3200_VD1/Software/sortmerna-2.0-linux-64/rRNA_databases/silva-bac-16s-id90.fasta,/media/MD3200_VD1/Software/sortmerna-2.0-linux-64/index/silva-bac-16s-db:/media/MD3200_VD1/Software/sortmerna-2.0-linux-64/rRNA_databases/silva-arc-23s-id98.fasta,/media/MD3200_VD1/Software/sortmerna-2.0-linux-64/index/silva-arc-23s-db:/media/MD3200_VD1/Software/sortmerna-2.0-linux-64/rRNA_databases/silva-arc-16s-id95.fasta,/media/MD3200_VD1/Software/sortmerna-2.0-linux-64/index/silva-arc-16s-db:/media/MD3200_VD1/Software/sortmerna-2.0-linux-64/rRNA_databases/rfam-5s-database-id98.fasta,/media/MD3200_VD1/Software/sortmerna-2.0-linux-64/index/rfam-5s-db:/media/MD3200_VD1/Software/sortmerna-2.0-linux-64/rRNA_databases/rfam-5.8s-database-id98.fasta,/media/MD3200_VD1/Software/sortmerna-2.0-linux-64/index/rfam-5.8s-db --reads /media/MD3200_VD1/Eva/bahiaF/sortmerna_results/ CGB113-1_merge_pear.fastq --sam --num_alignments 1 -e 0.00001 --fastx --paired_out --aligned ./CGB113-1_rRNA --other ./CGB113-1_non_rRNA -a 10 --log &
```

#### Assembly reads using [Spades-3.6.1](https://cab.spbu.ru/software/spades/)
```
SPAdes-3.6.1-Linux/bin/spades.py --careful --only-assembler --s1 ../CGB113-1_norRNA.fastq -o ./CGB113-1_nonrRNA_spades -t 12 -m 200 &

```
#### Keep contigs >200pb

```
cat final_contigs.fasta |awk '$0 ~ ">" {if (NR > 1) {print c;} c=0;printf substr($0,2,100) "\t"; } $0 !~ ">" {c+=length($0);} END { print c; }' 
seqtk subseq final_contigs.fasta final_contigs_size_more_200.fasta > file_more200.fasta

```

#### ORFs prediction
```
MetaGeneMark_linux_64/mgm/gmhmmp -a -d -f G -m mgm/MetaGeneMark_v1.mod -o ./CGB113-1_meta_gen_mark_lst /home/mario/Documentos/final_contigs_size_more_200.fasta -A prot_seq_CGB113-1.gff -D nucl_seq_CGB113-1.gff
```
