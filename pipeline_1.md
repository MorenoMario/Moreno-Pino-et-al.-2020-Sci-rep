## Reads processing

#### Read trimming
```
e.g.

```


#### Read trimming

```
e.g.
cutadapt -b AGATCGGAAGAGCACACGTCTGAACTCCAGTCA -b AGATCGGAAGAGCGTCGTGTAGGGAAAGAGTGT CGB113-1_rRNA_R1.fastq -q 30 -O 6 -o CGB113-1_rRNA_R1_cutadapt.fastq

```
#### Merge read

```
e.g.
pear-0.9.6-bin-64 -v 20 -f CGB113-1_rRNA_R1_cutadapt.fastq -r CGB113-1_rRNA_R2_cutadapt.fastq -o CGB113-1_merge_pear.fastq
```


