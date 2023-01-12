# sbash_script

#!/bin/bash

SECONDS=0  

#STEP 1: Run fastqc

cd /truba/home/sbuyukkilic/fastqc_tool/Analysis_test  #location of fastq files

/truba/home/sbuyukkilic/sequana/FastQC/fastqc SRR22016661.fastq

#check quality of fastq file look at html file

#run trimmomatic for trim low quality of bases

java -jar /truba/home/sbuyukkilic/trimmomatic/Trimmomatic-0.39/trimmomatic-0.39.jar PE SRR22016661_1.fastq.gz SRR22016661_2.fastq.gz SRR22016661_1_trimmed.fastq SRR22016661_1.untrimmed.fastq SRR22016661_2
_2.trimmed.fastq SRR22016661_2.untrimmed.fastq ILLUMINACLIP:TruSeq3-PE.fa:2:30:10:2:True LEADING:3 TRAILING:3 MINLEN:3

echo "Trimmomatic finished running!"
#Run fastqc again

/truba/home/sbuyukkilic/sequana/FastQC/fastqc SRR22016661_1_trimmed.fastq SRR22016661_2.trimmed.fastq

# STEP 2: Run STAR

/truba/home/sbuyukkilic/STAR-2.7.10b/bin/Linux_x86_64_static/./STAR --runThreadN 150 --genomeDir /truba/home/sbuyukkilic/STAR --readFilesIn SRR22016661_1.fastq.gz SRR22016661_2.fastq.gz --outFileNamePrefix ./align/SRR22016661_1.fastq.gz SRR22016661_2.fastq.gz –outBAMtype BAM SortedByCoordinate –quantMode GeneCounts

# STEP 3: Run featureCounts - Quantification

/truba/home/sbuyukkilic/fastqc_tool/subread-1.5.3-Linux-x86_64/bin/featureCounts -a /truba/home/sbuyukkilic/files/gencode.v42.annotation.gtf -o /truba/home/sbuyukkilic/fastqc_tool/readCount/SRR22016661_counts.txt /truba/home/sbuyukkilic/fastqc_tool/align/SRR22016661_1.fastqAligned.out.sam


duration=$SECONDS
echo "$(($duration / 60)) minutes and $(($duration % 60)) seconds elapsed."
# work on R 
