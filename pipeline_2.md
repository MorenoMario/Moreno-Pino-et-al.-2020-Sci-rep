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
awk '!/^@/ && $3!="*" {x="\t"; print $3 x $4 x length($10)+$4 x $1}' CGB113-2.sam  | sortBed -i - >CGB113-2.bed
fastalength ../../nucl_seq_rename_CGB113-2.fasta | awk '{print $2"\t"$1}' > contig_length.tab

bedtools genomecov -i CGB113-2.bed -g contig_length.tab | cut -f1,2 | perl max.perl - >cov_max_CGB113-2.tab

#script para obtener la cov mas larga por ORF
%max = ();
while(<>) {
        chomp $_;
        my ($gene,$cov) = split("\t",$_);
        if(!$max{$gene}) {
                $max{$gene} = $cov;
        }
        if($max{$gene} < $cov) {
                $max{$gene} = $cov;
        }
}
foreach my $gene (keys %max) {
        print "$gene\t$max{$gene}\n";
}

cut -f2 -d "_" cov_max_CGB113-2.tab | sort -n -k1 | awk '{print "gene_"$0}' >cov_max_CGB113-2_sorted.tab

awk '{sum+=$2} END {print sum}'  cov_max_CGB113-1_sorted.tab | less

```
