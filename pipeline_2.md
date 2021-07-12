### Annotations of ORFs for each metagenome

#### Annotation against Eggnog db 
```
nohup diamond blastp -p 12 -d eggnog4.dmnd -q  nucl_seq_rename_CGB113-1_fix.fasta -a ./eggNOG_prot_CGB113-2 -k 10 -e 0.00001 &

```
#### Annotation against Pfam db 

```
hmmsearch --tblout ./CGB113-1_hmm_pfam -E 1e-5 --cpu 12 media/storage1/data_bases/pfam/Pfam-A.hmm ../../ORF/prot_seq_rename_CGB113-1_fix.fasta

```
#### Mapping reads against contigs to obtain abundance of seq. using Bowtie2

```
bowtie2 --local --very-sensitive-local -q -x /media/storage1/metagenoma_CGB113/mario_sponge/cutadapt_seq_good_pandaseq/only_assammble_analysis/fan_moreno/spades_correcto_diciembre/CGB113-2/ORF/bowtie/end-to-end/nucl_seq_rename_CGB113-2.fasta -1 /media/storage1/metagenoma_CGB113/mario_sponge/cutadapt_seq_good_pandaseq/only_assammble_analysis/fan_moreno/sortmerna_correcto_diciembre/CGB113-2_R1_norRNA.fastq -2 /media/storage1/metagenoma_CGB113/mario_sponge/cutadapt_seq_good_pandaseq/only_assammble_analysis/fan_moreno/sortmerna_correcto_diciembre/CGB113-2_R2_norRNA.fastq -S CGB113-2.sam -p 12

samtools view -bS CGB113-1.sam > CGB113-1.bam
samtools view -hf 0x2 CGB113-1.bam | grep -v "XS:i:" | ./foo.py > CGB113-1.filtered.alignments.sam
awk '!/^@/ && $3!="*" {x="\t"; print $3 x $4 x length($10)+$4 x $1}' CGB113-1.filtered.alignments.sam | sortBed -i - >CGB113-1.filtered.alignments.bed
cut -f1,4 CGB113-1.filtered.alignments.bed | sort -u | cut -f1 | sort | uniq -c | awk '{print $2"\t"$1}' >CGB113-1.filtered.alignments.tab
./merge_list CGB113-2.filtered.alignments.tab orf_length.tab > CGB113-2.filtered.alignments_orf_length.tab

```
