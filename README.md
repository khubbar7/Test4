# Test4
Test4

## STAR runs!

Changing into and making the correct directories

```

cd /lustre/isaac/proj/UTK0208/test4/analysis/
mkdir khubbar7
cd khubbar7
mkdir star_2
cd star_2

```

linking in all the files ill need

```
ln -s  /lustre/isaac/proj/UTK0208/test4/raw_data/*.fastq .
```

creating a list of the name of the files because array only deals with numbers 

```
ls *fastq > filenames.txt

```


open nano and write (save as starscript.txt):

```
#!/bin/bash
#SBATCH -J star-mapTA
#SBATCH --nodes=1
#SBATCH --ntasks=8
#SBATCH -A ISAAC-UTK0208
#SBATCH -p condo-epp622
#SBATCH -q condo
#SBATCH -t 02:00:00
#SBATCH --mem-per-cpu=8G
#SBATCH --array=1-12
#SBATCH --mail-type=BEGIN,END,FAIL
#SBATCH --mail-user=khubbar7@utk.edu

module load star

readonefile=$(sed -n -e "${SLURM_ARRAY_TASK_ID} p" filenames.txt)
echo "readonefile is $readonefile"

readtwofile=$(echo $readonefile | sed 's/_1.fastq/_2.fastq/')
echo "readtwofile is $readtwofile"

outprefix=$(echo $readonefile | sed 's/_1.fastq//')
echo "outprefix is $outprefix"

time \
STAR \
--genomeDir /lustre/isaac/proj/UTK0208/rnaseq/raw_data/STAR_gtf_idx \
--runThreadN 8 \
--readFilesIn $readonefile $readtwofile \
--readFilesCommand cat \
--outFileNamePrefix $outprefix \
--outSAMtype BAM SortedByCoordinate \
--quantMode TranscriptomeSAM GeneCounts

```


run!!!

```
sbatch starscript.txt

```

check to make sure youre still jelly:

```
squeue -u khubbar7

```

| Name                         |   # of reads     |
| ---------------------------- | ---------------- |
| Badami_LB_400CH_Clone14      | 23114385         |
| Badami_LB_400CH_Clone15      | 21361266 |
| Badami_LB_400CH_Clone16      | 20521394 | 
| Badami_LB_800CH_Clone14      | 21748763 |
| Badami_LB_800CH_Clone15      | 23354376 |
| Badami_LB_800CH_Clone16      | 23243567 |
| Bergeron_LB_400CH_Clone10    | 19242671 |
| Bergeron_LB_400CH_Clone8     | 20217335 |
| Bergeron_LB_400CH_Clone9     | 19858725 |
| Bergeron_LB_800CH_Clone10    | 21645036 |
| Bergeron_LB_800CH_Clone8     | 20054302 |
| Bergeron_LB_800CH_Clone9     | 21650013 |

##STAR								
|    	NAME                 | Uniquely mapped reads %	| % of reads mapped to multiple loci	| % of reads mapped to too many loci	| % of reads unmapped: too many mismatches	| % of reads unmapped: too short	| % of reads unmapped: other	| % of chimeric reads |	
| ------------------------ | -----------------------	| ----------------------------------	| ----------------------------------	| ----------------------------------------	| ------------------------------	| --------------------------	| ------------------- |
Badami_LB_400CH_Clone14    |	0.00 | 1.82 | 2.08 | 0.00 | 86.22 | 9.88 | 0.00 |
Badami_LB_400CH_Clone15		 |	0.00 | 0.91 | 0.70 | 0.00 | 96.92 | 1.47 | 0.00|
Badami_LB_400CH_Clone16		 |  0.00 | 0.98 | 0.39 | 0.00 | 95.51 | 3.12 | 0.00 | 
Badami_LB_800CH_Clone14		 | 0.00 | 1.32 | 1.34 | 0.00 | 95.71 | 1.62 | 0.00 |
Badami_LB_800CH_Clone15		 |	0.00 | 0.99 | 0.70 | 0.00 | 97.28 | 1.03 | 0.00 |
Badami_LB_800CH_Clone16		 |	0.00 | 1.02 | 0.48 | 0.00 | 95.50 | 3.00 | 0.00 |
Bergeron_LB_400CH_Clone10	 |  0.00 | 1.31 | 1.75 | 0.00 | 95.15 | 1.80 | 0.00 |
Bergeron_LB_400CH_Clone8	 | 0.00 | 0.90 | 0.44 | 0.00 | 96.32 | 2.35 | 0.00 |
Bergeron_LB_400CH_Clone9	 |	0.00 | 1.00 | 0.52 | 0.00 | 94.40 | 4.07 | 0.00 | 			
Bergeron_LB_800CH_Clone10	 | 0.00 | 1.59 | 0.77 | 0.00 | 91.82 | 5.82 | 0.00 |
Bergeron_LB_800CH_Clone8	 | 0.00 | 0.97 | 0.49 | 0.00 | 97.97 | 0.65 | 0.00 | 		
Bergeron_LB_800CH_Clone9	 |	0.00 | 1.11 | 0.69 | 0.00 | 94.49 | 3.71 | 0.00 |		

```
grep -n -e  Prupe.6G364900 -e Prupe.1G531100 -e Prupe.1G531400 -e Prupe.1G549600 Badami_LB_400CH_Clone14.fastqReadsPerGene.out.tab ; grep -n -e  Prupe.6G364900 -e Prupe.1G531100 -e Prupe.1G531400 -e Prupe.1G549600 Badami_LB_400CH_Clone15.fastqReadsPerGene.out.tab ; grep -n -e  Prupe.6G364900 -e Prupe.1G531100 -e Prupe.1G531400 -e Prupe.1G549600 Badami_LB_400CH_Clone16.fastqReadsPerGene.out.tab ; grep -n -e  Prupe.6G364900 -e Prupe.1G531100 -e Prupe.1G531400 -e Prupe.1G549600 Badami_LB_800CH_Clone14.fastqReadsPerGene.out.tab ; grep -n -e  Prupe.6G364900 -e Prupe.1G531100 -e Prupe.1G531400 -e Prupe.1G549600 Badami_LB_800CH_Clone15.fastqReadsPerGene.out.tab ; grep -n -e  Prupe.6G364900 -e Prupe.1G531100 -e Prupe.1G531400 -e Prupe.1G549600 Badami_LB_800CH_Clone16.fastqReadsPerGene.out.tab 


grep -n -e  Prupe.6G364900 -e Prupe.1G531100 -e Prupe.1G531400 -e Prupe.1G549600 Bergeron_LB_400CH_Clone10.fastqReadsPerGene.out.tab ; grep -n -e  Prupe.6G364900 -e Prupe.1G531100 -e Prupe.1G531400 -e Prupe.1G549600 Bergeron_LB_400CH_Clone8.fastqReadsPerGene.out.tab ; grep -n -e  Prupe.6G364900 -e Prupe.1G531100 -e Prupe.1G531400 -e Prupe.1G549600 Bergeron_LB_400CH_Clone9.fastqReadsPerGene.out.tab ; grep -n -e  Prupe.6G364900 -e Prupe.1G531100 -e Prupe.1G531400 -e Prupe.1G549600 Bergeron_LB_800CH_Clone10.fastqReadsPerGene.out.tab ; grep -n -e  Prupe.6G364900 -e Prupe.1G531100 -e Prupe.1G531400 -e Prupe.1G549600 Bergeron_LB_800CH_Clone8.fastqReadsPerGene.out.tab ; grep -n -e  Prupe.6G364900 -e Prupe.1G531100 -e Prupe.1G531400 -e Prupe.1G549600 Bergeron_LB_800CH_Clone9.fastqReadsPerGene.out.tab

```

| Name       |  Prupe.6G364900  | Prupe.1G531100  |  Prupe.1G531400 | Prupe.1G549600  |
| ---------- | ---------- | ----------| -------- | ----------- |
| Badami_LB_400CH_Clone14      |   0 | 0 | 0 | 0 |    
| Badami_LB_400CH_Clone15      | 0 | 0 | 0 | 0 | 
| Badami_LB_400CH_Clone16      | 0 | 0 | 0 | 0 |
| Badami_LB_800CH_Clone14      | 0 | 0 | 0 | 0 |
| Badami_LB_800CH_Clone15      | 0 | 0 | 0 | 0 |
| Badami_LB_800CH_Clone16      | 0 | 0 | 0 | 0 |
| Bergeron_LB_400CH_Clone10    | 0 | 0 | 0 | 0 |
| Bergeron_LB_400CH_Clone8     | 0 | 0 | 0 | 0 |
| Bergeron_LB_400CH_Clone9     | 0 | 0 | 0 | 0 |
| Bergeron_LB_800CH_Clone10    | 0 | 0 | 0 | 0 |
| Bergeron_LB_800CH_Clone8     | 0 | 0 | 0 | 0 |
| Bergeron_LB_800CH_Clone9     | 0 | 0 | 0 | 0 |



## Now Salmon!

moving there 

```
cd ../
makedir salmon
cd salmon
```

making an array txt file

```
ls *fastq > filenames.txt
```

In nano editor:

```
nano salmon.qsh
```




```
#!/bin/bash
#SBATCH -J salmonTA
#SBATCH --nodes=1
#SBATCH --ntasks=4
#SBATCH -A ISAAC-UTK0208
#SBATCH -p condo-epp622
#SBATCH -q condo
#SBATCH -t 00:20:00
#SBATCH --mem-per-cpu=4G
#SBATCH --array=1-12

readonefile=$(sed -n -e "${SLURM_ARRAY_TASK_ID} p" filenames.txt)
echo "readonefile is $readonefile"
readtwofile=$(echo $readonefile | sed 's/_1.fastq/_2.fastq/')
echo "readtwofile is $readtwofile"
quantdir=$(echo $readonefile | sed 's/_1.fastq/_quant/')
echo "quantdir is $quantdir"

time \
/lustre/isaac/proj/UTK0208/rnaseq/software/salmon-1.9.0_linux_x86_64/bin/salmon \
quant \
-i /lustre/isaac/proj/UTK0208/rnaseq/raw_data/salmon_transcripts_index/ \
-l IU \
-1 $readonefile \
-2 $readtwofile \
--validateMappings \
-o $quantdir \
--threads 4

```

This is where i keep getting this error

Path ["Bergeron_LB_800CH_Clone10.fastq"] already exists and is not a directory.
Please either remove this file or choose another auxiliary directory.

I even tried to make a decoy file 
grep '^>' /lustre/isaac/proj/UTK0208/rnaseq/raw_data/salmon_transcripts_index/ | sed 's/>//' > decoys.txt
