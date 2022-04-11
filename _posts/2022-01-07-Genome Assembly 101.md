What is genome? The bibliography of your life.

**Genome assembly is to figure out the order of A,T,G,C.**

After sequencing, you get garbled characters. To assemble a book, first, we need to get each sentence from these characters first.second, we put the sentences into a paragraph, and then organize the paragraphs into an article, and then a chapter, a book. 
Sentence(variant calling) ->  paragraph(De Bruijn graph) -> article(sequence alignment) -> chapter(annotation) -> book(the genome)
(science starts from common sense!)

Techniques is easy to get, but when we want to get a genome, **the first question we should ask is "why we need to 'assemble' genome?, is it the best way to get genome?",**  rathe than  "what kind of methods we can use to assemble a genome", 
So we should start from the question, how we get genetic information? Why cannot we directly get the organized genome?
let's think about you want to download a e-book from web, which has 100 chapters, 1GB in total. However, your hard-drive can only store 200MB, so you have to download the book by one chapter after another chapter. Similarly, because of the technical limitation, the sequencing so far can only break down the genome into smaller pieces, each piece ranges from 100 base pair to 300 base pair. And the biomedical reading machine is not that accurate as the computers, so the problem is when you download the book(of genome), you always get garbled text and you don't know which chapter you're downloading.

So, maybe one day we can have revolution in the biotech sequencing, and then we can get the genome directly.

This blog would serve as a step-by-step tutorial for people interested in assemble a genome by themselves. After the methods, we will talk about what kind of problems these methods have.


## wget get the original sequencing file (format: fasta)
If using public dataset, we can use NCBI.
wget https://sra-pub-run-odp.s3.amazonaws.com/sra/SRR7811197/SRR7811197

```bash
genomics2021@ruderalis:~/student_folders/yutigao/finalplroject$ wget https://sra-pub-run-odp.s3.amazonaws.com/sra/SRR7811197/SRR7811197
--2021-09-23 13:13:09--  https://sra-pub-run-odp.s3.amazonaws.com/sra/SRR7811197/SRR7811197

Resolving sra-pub-run-odp.s3.amazonaws.com (sra-pub-run-odp.s3.amazonaws.com)... 52.216.108.131
Connecting to sra-pub-run-odp.s3.amazonaws.com (sra-pub-run-odp.s3.amazonaws.com)|52.216.108.131|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 4057727206 (3.8G) [application/x-troff-man]
Saving to: ‘SRR7811197’

SRR7811197       100%[=========>]   3.78G  3.43MB/s    in 12m 0s  
```



# fastq-dump --split -3 (files)

> --split-3 separates the reads into left and right ends. If there is a left end without a matching right end, or a right end without a matching left end, they will be put in a single file.
> [bioinformatics notebook] (https://rnnh.github.io/bioinfo-notebook/docs/fastq-dump.html)
> DNA is double-helixed, read from left to right, and then right to left
> dump, Database dump, usually means a record of the table structure and/or the data from a database

fastq-dump --split-3 SRR7811197

```bash
genomics2021@ruderalis:~/student_folders/yutigao/finalplroject$ fastq-dump --split-3 SRR7811197 

bg
^C2021-09-23T19:28:27 fastq-dump.2.8.2 sys: libs/kns/unix/syssock.c:492:KSocketTimedRead: transfer interrupted while reading file within network system module - mbedtls_ssl_read returned -26880 ( SSL - Connection requires a read call )
2021-09-23T19:28:28 fastq-dump.2.8.2 err: libs/kapp/unix/sysmain.c:75:Quitting: process canceled while executing process - failed SRR7811197
2021-09-23T19:28:28 fastq-dump.2.8.2 warn: libs/kproc/sem.c:207:KSemaphoreTimedWait: timeout exhausted while waiting semaphore within process system module - libs/kproc/sem.c:207 within KSemaphoreTimedWait

genomics2021@ruderalis:~/student_folders/yutigao/finalplroject$ ls
SRR1868103          SRR1868103_2.fastq  SRR7811197_2.fastq
SRR1868103.fastq    SRR7811197
SRR1868103_1.fastq  SRR7811197_1.fastq
```

--split-3 separates the reads into left and right ends. so we get SRR7811197_1.fastq，SRR7811197_2.fastq 

check whether the 2 fastq has same length

```bash
genomics2021@ruderalis:~/student_folders/yutigao/finalplroject$ wc -l *fastq
  11039012 SRR7811197_1.fastq
  11039012 SRR7811197_2.fastq
```

# fastqc: quality control

##  fastqc SRR7811197_1.fastq, fastqc SRR7811197_2.fastq

```bash
genomics2021@ruderalis:~/student_folders/yutigao/finalplroject$ fastqc SRR7811197_1.fastq
Started analysis of SRR7811197_1.fastq
Approx 5% complete for SRR7811197_1.fastq
Approx 10% complete for SRR7811197_1.fastq
Approx 15% complete for SRR7811197_1.fastq
Approx 20% complete for SRR7811197_1.fastq
Approx 25% complete for SRR7811197_1.fastq
Approx 30% complete for SRR7811197_1.fastq
Approx 35% complete for SRR7811197_1.fastq
Approx 40% complete for SRR7811197_1.fastq
Approx 45% complete for SRR7811197_1.fastq
Approx 50% complete for SRR7811197_1.fastq
Approx 55% complete for SRR7811197_1.fastq
Approx 60% complete for SRR7811197_1.fastq
Approx 65% complete for SRR7811197_1.fastq
Approx 70% complete for SRR7811197_1.fastq
Approx 75% complete for SRR7811197_1.fastq
Approx 80% complete for SRR7811197_1.fastq
Approx 85% complete for SRR7811197_1.fastq
Approx 90% complete for SRR7811197_1.fastq
Approx 95% complete for SRR7811197_1.fastq
Analysis complete for SRR7811197_1.fastq
```

得到.html .zip 文件，网页是评估的结果

```bash
genomics2021@ruderalis:~/student_folders/yutigao/finalplroject$ ls
SRR1868103          SRR1868103_1_fastqc.html  SRR7811197
SRR1868103.fastq    SRR1868103_1_fastqc.zip   SRR7811197_1.fastq
SRR1868103_1.fastq  SRR1868103_2.fastq        SRR7811197_2.fastq
```

将文件从服务器scp 到本地进行查看

```bash
(base) GaoYutingdeMacBook-Air:~ gaoyuting$ scp genomics2021@ruderalis.colorado.edu:~/student_folders/yutigao/finalplroject/SRR1868103_1_fastqc.html ./
genomics2021@ruderalis.colorado.edu's password: 
SRR1868103_1_fastqc.html         100%  720KB   2.1MB/s   00:00    
```

![fastqc 网页](/Users/gaoyuting/Pictures/fatsqc.png)

sequence quality in green area means good, if there are some in red area, we need to Trimmomatic

# Trimmomatic, drop out the bad quality data 

trim v.
remove the edges from and cut down to the desired size
[Trimmomatic: A flexible read trimming tool for Illumina NGS data](http://www.usadellab.org/cms/?page=trimmomatic)

## Trimmomatic commands:

Paired End Mode:
java -jar <path to trimmomatic.jar> PE [-threads <threads] [-phred33 | -phred64] [-trimlog <logFile>] <input 1> <input 2> <paired output 1> <unpaired output 1> <paired output 2> <unpaired output 2> <step 1> ...

What does these command lines do?

> Trimmomatic is a fast, multithreaded command line tool that can be used to trim and cropIllumina (FASTQ) data as well as to remove adapters.  adapter is a short piece of sequence that sequencing company add during sequencing, like the fishing bait. Now we need to remove it from the genomic data we get.

> The paired end mode will maintain correspondence of read pairs and also use the additional
> information contained in paired reads to better find adapter or PCR primer fragments
> introduced by the library preparation process

> phred + 33 or phred + 64 是quality scores, depending on the Illumina pipeline used

```bash
genomics2021@ruderalis:~/student_folders/nolan_kane$ cat command.txt
java -jar ~/trimmomatic-0.39.jar PE -phred33 -trimlog log SRR1868103_1.fastq  SRR1868103_2.fastq SRR1868103_1_paired.fq SRR1868103_1_upaired.fq SRR1868103_2_paired.fq SRR1868103_2_upaired.fq ILLUMINACLIP:TruSeq3-PE.fa:2:20:10 LEADING:20 TRAILING:20 SLIDINGWINDOW:4:15:15 MINLEN:100
```

output: paired.fq unpaired.fq, trim data 的目的是为提高采用的数据的质量, 所以再次fastqc 

```bash
genomics2021@ruderalis:~/student_folders/yutigao/finalproject$ bash trimmomatic.sh
genomics2021@ruderalis:~/student_folders/yutigao/finalproject$ ls
SRR7811197_1_paired_fastqc.zip
SRR7811197_1_unpaired.fq
SRR7811197_2.fastq
SRR7811197_2_paired.fq
SRR7811197_1_paired_fastqc.html
SRR7811197_2_paired_fastqc.html
SRR7811197_2_paired_fastqc.zip
SRR7811197_2_unpaired.fq
SRR7811197_1.fastq           
```

# Pipeline: bwa + samtools + bcftools + grep & awk 

align to reference genome 
bwa, samtools, bcftools are bioinformatics program well written and contained

Program: bwa (alignment via Burrows-Wheeler transformation)
Version: 0.7.17-r1188
Contact: Heng Li <lh3@sanger.ac.uk>
Usage:   bwa <command> [options]

Program: samtools (Tools for alignments in the SAM format)
Version: 1.13 (using htslib 1.13)
Usage:   samtools <command> [options]

bcftools
About:   SNP/indel variant calling from VCF/BCF. To be used in conjunction with bcftools mpileup.
         This command replaces the former "bcftools view"
         Usage:   bcftools call [options] <in.vcf.gz>
         

```bash
# bwa index .fa
# bwa mem .fa .fastq > .sam
# samtools view -b -o .bam
# samtools sort .bam > sorted.bam
# samtools index sorted.bam
# samtools faidx sorted.bam
# bcftools mpileup -f .fa .bam| bcftools call -mv -o .vcf
# grep '' .vcf > .txt
# awk '$6>100{print$2,$6}' > goodsnps.txt
```

with annotation:

```bash
### create bam
bwa index mt.fa
	# bwa index .fa file
	# index sequences in the FASTA format (end in .fa) and create file format in .bwt .pac. ann .amb .sa

bwa mem mt.fa sra_data.fastq > ler.sam
	# bwa mem .fa .fastq > .sam
	# .fasta: sequence
	# .fastq: with quality score

samtools view ler.sam -b -o ler.bam
	# view: SAM<->BAM<->CRAM conversion
	# -b: -bam output in bam format
	# -o: output file
	# -S: sort and read ler.sam output into ler.bam
	# or write in samtools view -b -o ler.bam -S ler.sam

samtools sort ler.bam -o ler.sorted.bam
	# sort bam file

samtools index ler.sorted.bam
	# index alignment

samtools faidx mt.fa
	# fasta index index/ extract fasta
	# creat mt.fa.fai

### make vcf using bcftools
bcftools mpileup -f mt.fa ler.sorted.bam |bcftools call -mv -o ler.vcf
	# save to output file of .bam to
	# multi-way pileup producing genotype likelihoods
	# -f: fasta-ref FILE
	# example: bcftools mpileup -Ou -f reference.fa alignments.bam | bcftools call -mv -Ob -o calls.bcf
	# Ou -- unpressed
	# Ob -- be binary (vcf is text file, bcf is binary file)
	# -m --multiallelic-caller     Alternative model for multiallelic and rare-variant calling (conflicts with -c)
	# -v --Output variant sites only

### grep and awk
grep -v '##' ler.vcf > ler_snps_indels.txt
	# delete ## columns
grep -v 'INDEL' ler.vcf > snps.txt
	# only snps
grep 'INDEL' ler.vcf > indels.txt
awk '$6>100{print$2,$4,$5,$6}' > goodsnps.txt
awk '$6>100{print$2,$4,$5,$6}' indels.txt > goodindels.txt
```

# awk 

# printing in awk: condition{action}

 awk '$6>100{print $4,$5,$6}' snps.txt
also can be:
 awk '$4 == “A” {print $0}' ler.vcf

 In awk, 1 means true and 0 not-true.
  If you type the command below, awk will print all the lines from the file you have selected.
  $ awk ‘1 {print $0}’ ler.vcf (in this case you can remove the default in this case for example {print $0},
  and awk will print the same list.

make the pipeline reproducible

```bash
ref = organelles.fa
fastq = sra_data.fastq
name = ler
bwa index $ref
bwa mem $ref $fastq > $name.sam
samtools view -b -o $name.bam -S $name.sam
samtools sort $name.bam > $name.sorted.bam
samtools i ndex $name.sorted.bam
samtools faidx $ref
bcftools mpileup -f $ref $name.sorted.bam |bcftools call -mv -o $name.vcf
grep -v '##' $name.vcf > ${name}_snps_indels.txt
grep -v 'INDEL' $name.vcf > snps.txt
grep 'INDEL' $name.vcf > indels.txt
awk '$6>100{print $1,$2,$4,$5,$6}'${name}_snps_indels.txt
 > goodsnps.txt
```

# de novo assembly (sentence -> paragraph) 

de novo, Latin, literally ‘from new"

## De Bruijn Graph


De Bruijn Graph is way to show the overlap between different objects. Different order of ATGC would have different overlap and distance, De Bruijn graph would pick a best path based on these overlap and distance. Then we get the best contig, which ideally should be the original piece before the sequencing beaks it down.

# SPAdes 

[software SPAdes](https://cab.spbu.ru/software/spades/)

SPAdes is a bioinformatic program based on De Bruijn Graph, which divide the SNPs into K base pairs nucleotides, called K-mer, and then construct the scaffold based on the overlap between K-mer.

Rule: K must be odd.
Why?
For example, K-mer = 17, ATGGGGGCTCTCGAAAA.
DNA is double-helixed, so SPAdes read it twice in different directions, 1st time from left to right, ATGGGGGCTCTCGAAAA, and then right to left, AAAAGCTCTCGGGGGTA, we can see the two reads are different.



```bash
head -n 4000000 SRR7811219_1_paired.fq > subset_1.fq
head -n 4000000 SRR7811219_2_paired.fq > subset_2.fq

head -n 400000 mapped_1.fq > subset_mapped_1.fq
head -n 400000 mapped_2.fq > subset_mapped_2.fq
```

## spades --phred-offset 33 --careful -k 21,33,45,55,77 -1 subset_mapped_1.fq -2 subset_mapped_2.fq -o spades4


![scaffold, contig](https://github.com/yuti-gao/yuti-gao.github.io/raw/b280a5aae91b04e56b099822e7383141528def18/_posts/Paired-end%20reads%20span%20gaps.png)

we get scaffold.fasta from SPAdes, which has different folds.

```bash
>NODE_1_length_61316_cov_90.563334
ACCAATTAATATCATTCAAATTTCTGATCTAAATCACATTGGAAAAAAAATTGTAAAAGC
ACTACGGAATCAAATAAAGGATTTAGAAAATGATAAGACTCATCTTCTCAATGAACGATT
GACTTTGATACAAGATAAGATAGAACAGTCTATTCTTCATCAACCTTCTCTATACAATAC
TAAGTCTGAGGCAAAGAAGGCTAGGAAAGCTAAGAAAAGAAATGTGGATAGCTCACCACA
ACTACCTAAACCTGATGAATAATAGAATTTGGAAATATAAAGTTTCTTGGCCAATTGAGC
GAAGCCAAGCCGTATAAGGGCGTGCAGCACGTATAGCGAGTCAAGGAATAGCAAACGGCC
···
>NODE_2_length_22313_cov_57.552258
GGAAGGTTGCTTTGTTACCTATTCCACTTGGAACCGCGGATTTTTTGGTCCATCACATTC
ATGCATTTACGATTCATGTGACGGTATTGATACTCCTAAAGGGAGTTCTATTTGCCCGCA
GCTCTCGTTTGATACCAGATAAAGCAAATCTGGGTTTTCGCTTCCCTTGTGATGGACCTG
GAAGAGGGGGGACATGTCAAGTATCCGCTTGGGATCATGTCTTCTTAGGATTATTTTGGA
```

length, # of nucleotides
cov, coverage, how deep the sequencing is 

## put nodes in order 

remove node title from scaffold.fasta，align to reference genome in [NCBI blast](https://blast.ncbi.nlm.nih.gov/Blast.cgi)

![nodes](https://github.com/yuti-gao/yuti-gao.github.io/raw/ae639ff5e1a8d9cf9dca3637980bf8206e4d61b8/_posts/nodes.png)

put fasta in order according to the nodes shown here 
鼠标光标会显示在依次显示的是第几个node, 我们需要将fasta 按照这里的node的顺序重新排列

compare the reference choloplast genome(query genome) with my genome (subject genome)
reorder the nodes sequence from scaffolds according to the graphic summary
make a dotplot
![dotplot](https://github.com/yuti-gao/yuti-gao.github.io/raw/abcf2e910b397a48a518d682c685628985e10021/_posts/dot%20plot.png)(by the way，insert picture in github page，see https://qcow.cc/common/2021/02/23/HowToUsePictureOnGithubPages/ put picture in github page repository, copy permalink, https://github.com/yuti-gao/yuti-gao.github.io/blob/abcf2e910b397a48a518d682c685628985e10021/_posts/dot%20plot.png, follow the picture syntax ![]()),change blob permalink into raw )

# error correction 

"ALL genomes have mistakes", all algorithms have errors.
The latest human genome 38th version, still has 
350 gaps.

error correction is most difficult 

## Identifying errors

• Align the reads back to the assembly
• Comparison to other genomes or genomic resources
see the tview, to see the quality of the genome we put together 

To correct your genome we will use two approaches.

1. Use zpicture to identify potential errors in the over-all structure. Make sure you do that and understand the results.
   [zpicture](http://zpicture.dcode.org)
2. Align the reads from your fastq files to make sure that the reads
   agree with your assembly. Make a new version of your pipeline that aligns the reads to your cloroplast assembly to identify possible errors -- vcf variant calling 

find the starting point (as circularized)
put the circularized as reference genome
run the SNP calling pipeline again

## reference genome 

[reference genome](https://www.ncbi.nlm.nih.gov/nuccore/KY849971.1)

Tips on finding reference genome: 

taxonomy -> genome -> organelle annotation report -> replicons

![reference genome](https://github.com/yuti-gao/yuti-gao.github.io/raw/eec0265d0a271befb1e8b124cba863e58f9d66e6/_posts/Screen%20Shot%202022-01-09%20at%209.38.26%20PM.png)



# gap filling 



```bash
I’m trying to fill this gap, below, which is 24 bases, between these two contigs.
GTCAAGAATTGGGGCCTCGCAATCACTATTTTATCTCATGCCTTTCTTCGTTCATGGTTC
GATATTCTGGTGTCCTAGGCGTAGAGGAACCACACCAATCCATCCCGAACTTGGTGGTTA
AACTCTACTGCGGTGACGATACTGTAGGGGAGGTCCTGCGGAAAAATAGCTCGACGCCAG
GATGATAAAAAACTTCACACCTCCCGTTCTTCATACTTTTTCAAAAA
(24bp gap)
>NODE_405_length_1788_cov_93.027698
AAATGGCTGAGGAGAGCAAAGGTTCCTTTTTTTGAGGGTACTTCGGGGAACAGATCCAGT
GGAGACGCAGTGGGGCCTGTAGCTCAGAGGATTAGAGCACGTGGCTACGAACCACGGTGT
CGGGGGTTCGAATCCCTCCTCGCCCACAACCGGCCCAAAAGGGAAGGGCCTTTCCCTCTG
GGGGTAGGAAAATCATGATCGGGATAGCGGACCAAAAGCTATGGAACTTGGGTGTGGGTC
TTTTGTCGAAATGGAATGGCCTTTTTCTTTATTATTTATCGTAAATGAGTGAAGCATTAC
ACATAGTATGCCCGTCCCCCATCAGCGTATTTTTTTGTTTTACGCGCCCGTAACTCTTCC
TCAGCCAGGATGGGGCAGAATAGCAGAGCAAGTACAAGTATTAGTAGCATAACAAAAACG
CGTTCCTCGTCATTAATATGTTTGCTCGCGGCAATTGTGGCCTCTCGGTAGAATCGATGA
CTGCATCTTTAGAATCGATGACTGCATCTTTGATGCACTGCTAGTACTAGTACATC
First I’ll try searching for the sequence at the end of the first contig, by going into my folder on the server (ruderalis.colorado.edu) and doing
grep TCATACTTTTTCAAAAA *fastq
Here to help you visualize it, I’ve highlighted that sequence
AACTCTACTGCGGTGACGATACTGTAGGGGAGGTCCTGCGGAAAAATAGCTCGACGCCAG
GATGATAAAAAACTTCACACCTCCCGTTCTTCATACTTTTTCAAAAA
(24bp gap)
>NODE_405_length_1788_cov_93.027698
AAATGGCTGAGGAGAGCAAAGGTTCCTTTTTTTGAGGGTACTTCGGGGAACAGATCCAGT
Anyway, the grep works, and I get back a lot of sequences. Here are the first few lines:
genomics2020@ruderalis:~/student_folders2/nolan/project/carn$ grep
TCATACTTTTTCAAAAA *fastq
mapped_2.fastq:GGTGACGATACTGTAGGGGAGGACCTGCGGAAAAATAGCTCGACGCCAGGATGAT
AAAAAACTTCACACCTCCCGTTCT                 AAAAA
mapped_2.fastq:                 AAAAAAGAAAAATAAAAAGGTCGTCTTATTTAAAACCC
CAATTATGACATCCCCTCTCTCCCACTTCACACCTCGGAACGCGCC
TCATACTTTTTCAAAAA
TCATACTTTTTCAAAAA
  
None of those sequences seem to fill the gap completely. What I am looking for is a read that has the sequence found at the beginning of node 405, and none of the reads have that. So, I have to try to extend my search. Let’s take that second read I got back from the grep, and try to search again, using the sequence from the end of it. Here’s the read, below, and I’ve highlighted the last bases I want to search for in red :
mapped_2.fastq: AAAAAAGAAAAATAAAAAGGTCGTCTTATTTAAAACCC CAATTATGACATCCCCTCTCTCCCACTT
Again, the grep works, and I get back a lot of sequences. Here is the first hit:
genomics2020@ruderalis:~/student_folders2/nolan/project/carn$ grep
CACACCTCGGAACGCGCC *fastq
SRR1868103.1.fastq:CACTT                  GTTCTTATAGAGAGAAAGTAGAGATAAA
GGCGCTTTGAAATCTTCTTAACCCGAAATGGCTGA
That read is good – if I search for the sequence in blue, it is near the beginning of my NODE_405 sequence
>NODE_405_length_1788_cov_93.027698
AAATGGCTGAGGAGAGCAAAGGTTCCTTTTTTTGAGGGTACTTCGGGGAACAGATCCAGT
GGAGACGCAGTGGGGCCTGTAGCTCAGAGGATTAGAGCACGTGGCTACGAACCACGGTGT
CGGGGGTTCGAATCCCTCCTCGCCCACAACCGGCCCAAAAGGGAAGGGCCTTTCCCTCTG
GGGGTAGGAAAATCATGATCGGGATAGCGGACCAAAAGCTATGGAACTTGGGTGTGGGTC
TTTTGTCGAAATGGAATGGCCTTTTTCTTTATTATTTATCGTAAATGAGTGAAGCATTAC
ACATAGTATGCCCGTCCCCCATCAGCGTATTTTTTTGTTTTACGCGCCCGTAACTCTTCC
TCAGCCAGGATGGGGCAGAATAGCAGAGCAAGTACAAGTATTAGTAGCATAACAAAAACG
CGTTCCTCGTCATTAATATGTTTGCTCGCGGCAATTGTGGCCTCTCGGTAGAATCGATGA
CTGCATCTTTAGAATCGATGACTGCATCTTTGATGCACTGCTAGTACTAGTACATC
What that means is that the new sequences I have got from my fastq file fill the gap! I’ll illustrate below how that works by lining them up. The way the second read wraps, it lines up with my node 405 sequence
End of node 50: GATGATAAAAAACTTCACACCTCCCGTTCTTCATACTTTTTCAAAAA First read from the fastq file:
TCATACTTTTTCAAAAAAAAAAAGAAAAATAAAAAGGTCGTCTTATTTAAAACCCCAATTATGACATCCCCTCTCTCCCACTTCACACCTCGGAACGCGCC
TCATACTTTTTCAAAAA
 CACACCTCGGAACGCGCC
 
CACACCTCGGAACGCGCC
 GGAGAGCAAAGGTTC
 
   Second read from the fastq file: AAATGGCTGAGGAGAGCAAAGGTTC
>NODE_405_length_1788_cov_93.027698 AAATGGCTGAGGAGAGCAAAGGTTCCTTTTTTTGAGGGTACTTCGGGGAACAGATCCAGT GGAGACGCAGTGGGGCCTGTAGCTCAGAGGATTAGAGCACGTGGCTACGAACCACGGTGT CGGGGGTTCGAATCCCTCCTCGCCCACAACCGGCCCAAAAGGGAAGGGCCTTTCCCTCTG
SRR1868103.1.fastq:CACTTCACACCTCGGAACGCGCCGTTCTTATAGAGAGAAAGTAGAGATAAAGGCGCTTTGAAATCTTCTTAACCCG
   Now, the last step is to join them all into one sequence, with only one copy wherever there is overlap
End of node 50: GATGATAAAAAACTTCACACCTCCCGTTCTTCATACTTTTTCAAAAAAAAAAAGAAAAATAAAAAGGTCGTCTTATTTAAAACCCCAATTATGACATCCCCTCTCTCCCACTTCACACCTCGGAACGCGCCGTTCTTATAGAGAGAAAGTAGAGATAAAGGCGCTTTGAAATCTTCTTAACCCG AAATGGCTGAGGAGAGCAAAGGTTCCTTTTTTTGAGGGTACTTCGGGGAACAGATCCAGT
GGAGACGCAGTGGGGCCTGTAGCTCAGAGGATTAGAGCACGTGGCTACGAACCACGGTGT
CGGGGGTTCGAATCCCTCCTCGCCCACAACCGGCCCAAAAGGGAAGGGCCTTTCCCTCTG
```



## summary of error correction
error rnn16
1.reference genome, look for rrn16 

2.copy rr16 sequence, blast rrn16 (reference genome) with my own genome 

3. alignment, find the whole piece of plus/plus，copy and find in my own genome fasta 

4.gap filling, terminal:  grep '' *fq 

5. fix the plus/plus，reverse complement, check the position of reverse complement(inverted repeat)

6. replace plus/minus with reverse complement

# genome annotation 

e.g. choloplast 

[chloroplast annotation](https://chlorobox.mpimp-golm.mpg.de/geseq.html)

check genbank file, and circular genome

![o](https://github.com/yuti-gao/yuti-gao.github.io/raw/812ad05fb7f8aade4e79645265125cdc3e8d65f9/_posts/1102_norinum_25_flax_SSR220_OGDRAW.jpg)

look at the reference genome paper, several bugs:

1.fragment

2.inverse repeat not identical 

3.tRNA annotated repeatedly 

Through annotation, we can double check our error correction

# submission

Warnings from NCBI genbank, banlkt, examines our error correction 

Some potential errors:

1) protein coding sequences contain internal stop codons and/or lack start/stop 

Check stop condon with https://web.expasy.org/translate/

2. postion in annotation 

functional annotation: compare the similarity  

1.peudogene 

Find the sequence of pseudogene https://www.bioinformatics.org/sms2/genbank_feat.html

2.RNA editing

manually add it up

# take home message 

1.get DNA pieces from sequencing 

2. graph algorithms show the order of nodes

3. align to "right" genome (reference genome)

4.variant calling tell us the differences are biological difference or error

5. for errors, grep search the SNPs near your error, do error correction and  gap filling 

6.annotation, find the protein function

