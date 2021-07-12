### Annotations of ORFs for each metagenome

#### Annotation against Eggnog db 
```
nohup diamond blastp -p 12 -d eggnog4.dmnd -q  nucl_seq_rename_CGB113-1_fix.fasta -a ./eggNOG_prot_CGB113-2 -k 10 -e 0.00001 &

```
