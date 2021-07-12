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
bowtie2 -f -x CGB113-2_final_contigs.fasta -1 CGB113-1_nonrRNA_30_R1.fasta -2 CGB113-2_nonrRNA_30_R2.fasta -S CGB113-2.sam

```
