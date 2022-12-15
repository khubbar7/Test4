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



# Everything below this line was added AFTER the deadline!!! Added 12/15/22 9:10am!!! Just FYI!!

So i honestly got really mad that it was bugging at readtwofile so I just deleted it. and I gave it a lot more power. 

```
nano qscript.qsh
```

In nano:

```
#!/bin/bash
#SBATCH -J salmonTA
#SBATCH --nodes=1
#SBATCH --ntasks=8
#SBATCH -A ISAAC-UTK0208
#SBATCH -p condo-epp622
#SBATCH -q condo
#SBATCH -t 00:40:00
#SBATCH --mem-per-cpu=8G
#SBATCH --array=1-12



readonefile=$(sed -n -e "${SLURM_ARRAY_TASK_ID} p" filenames.txt)
echo "readonefile is $readonefile"
quantdir=$(echo $readonefile | sed 's/.fastq/_quant/')
echo "quantdir is $quantdir"


time \
/lustre/isaac/proj/UTK0208/rnaseq/software/salmon-1.9.0_linux_x86_64/bin/salmon \
quant \
-i /lustre/isaac/proj/UTK0208/rnaseq/raw_data/salmon_transcripts_index \
-l IU \
--validateMappings \
-o $quantdir \
--threads 8

```

| Name | Number of mappings discarded because of alignment score | Number of fragments entirely discarded because of alignment score | Number of fragments discarded because they are best-mapped to decoys | Number of fragments discarded because they have only dovetail (discordant) mappings to valid targets | Mapping rate |
| -------------- | --------------------| ----------------| -------------- | -------------- | ----------------- |
| Badami_LB_400CH_Clone14      | 205,857,087  | 4,574,890 | 1,861,825 | 0 | 27.8655% |
| Badami_LB_400CH_Clone15      | 43,298,735 | 1,080,852 | 597,469 | 0 | 48.2626% |
| Badami_LB_400CH_Clone16      | 30,001,636 | 777,868 | 397,852 | 0 | 50.5475% |
| Badami_LB_800CH_Clone14      | 111,272,146 | 2,497,059 | 1,098,713 | 0 | 42.9052% |
| Badami_LB_800CH_Clone15      | 44,623,923 | 1,090,677 | 609,426 | 0 | 44.8319% |
| Badami_LB_800CH_Clone16      | 34,591,074 | 864,153 | 459,936 | 0 | 51.0876% |
| Bergeron_LB_400CH_Clone10    | 118,152,251 | 2,655,728 | 1,193,557 | 0 | 35.1606% |
| Bergeron_LB_400CH_Clone8     | 27,042,730 | 729,748 | 373,094 | 0 | 46.0375% |
| Bergeron_LB_400CH_Clone9     | 30,312,088 | 777,403 | 411,150 | 0 | 43.7757% |
| Bergeron_LB_800CH_Clone10    | 127,397,710 | 2,783,569 | 1,005,090 | 0 | 25.3279% |
| Bergeron_LB_800CH_Clone8     | 27,625,759 | 719,575 | 372,274 | 0 | 52.0612% |
| Bergeron_LB_800CH_Clone9     | 39,660,692 | 962,418 | 523,580 | 0 | 50.8857% |

And now thanks to a super handy article by UCDavis because reading those one at a time sucked....

```
for FOLDER in *_quant; 
do echo $FOLDER >> salmonpurpe.txt; 
grep -E 'Prupe.6G364900.1' $FOLDER/quant.sf >> salmonpurpe.txt; 
grep -E 'Prupe.1G531100.1' $FOLDER/quant.sf >> salmonprupe.txt; 
grep -E 'Prupe.1G531100.2' $FOLDER/quant.sf >> salmonprupe.txt; 
grep -E 'Prupe.1G531400.1' $FOLDER/quant.sf >> salmonpurpe.txt; 
grep -E 'Prupe.1G549600.1' $FOLDER/quant.sf >> salmonpurpe.txt; 
grep -E 'Prupe.1G549600.2' $FOLDER/quant.sf >> salmonpurpe.txt; 
grep -E 'Prupe.1G549600.3' $FOLDER/quant.sf >> salmonpurpe.txt
done
`

| Name       |  Prupe.6G364900  | Prupe.1G531100  |  Prupe.1G531400 | Prupe.1G549600  |
| ---------- | ---------- | ----------| -------- | ----------- |
| Badami_LB_400CH_Clone14      |   88.000 | 208.867 | 0.000 | 1.000 | 0.00 |
| Badami_LB_400CH_Clone15      | 132.000 | 341.264 | 0.000 | 6.000 | 0.000 |
| Badami_LB_400CH_Clone16      | 156.000 | 310.593 | 0.000 | 0.000 | 0.000 |
| Badami_LB_800CH_Clone14      | 37.000 | 84.257 | 3.084  | 0.000 | 4.000 |
| Badami_LB_800CH_Clone15      | 20.000 | 61.868 | 0.000 | 0.000 | 4.000 | 
| Badami_LB_800CH_Clone16      | 53.000 | 86.996 | 1.915 | 0 | 12.085 |
| Bergeron_LB_400CH_Clone10    | 73.000 | 213.830 | 8.000 | 0.000 | 0.000 |
| Bergeron_LB_400CH_Clone8     | 145.000 | 303.083 | 6.003 | 0.000 | 0.000 |
| Bergeron_LB_400CH_Clone9     | 111.000 | 264.590 | 0.000 | 0.000 | 3.000 |
| Bergeron_LB_800CH_Clone10    | 2.000 | 26.555 | 3.000 | 0.000 | 0.000 |
| Bergeron_LB_800CH_Clone8     | 24.000 | 97.903 | 12.829 | 0.000 | 6.175 |
| Bergeron_LB_800CH_Clone9     | 62.000 | 96.859 | 15.214 | 23.786 | 0.000 |


## Comparing reads

Are more reads overall being mapped by STAR or Salmon? For the two genes with more than one transcript, is there a dominant transcript across samples? 

```
grep 'Mapping rate' slurm*
grep 'Uniquely mapped reads %' ../star_2/*Log.final.out

```

From the terminal

[khubbar7@login1 salmon]$ grep 'Mapping rate' slurm*
slurm-313513_10.out:[2022-12-15 09:11:24.711] [jointLog] [info] Mapping rate = 25.3279%
slurm-313513_11.out:[2022-12-15 09:11:29.226] [jointLog] [info] Mapping rate = 52.0612%
slurm-313513_12.out:[2022-12-15 09:11:35.899] [jointLog] [info] Mapping rate = 50.8857%
slurm-313513_1.out:[2022-12-15 09:10:28.241] [jointLog] [info] Mapping rate = 27.8655%
slurm-313513_2.out:[2022-12-15 09:09:47.721] [jointLog] [info] Mapping rate = 48.2626%
slurm-313513_3.out:[2022-12-15 09:09:43.649] [jointLog] [info] Mapping rate = 50.5475%
slurm-313513_4.out:[2022-12-15 09:10:05.243] [jointLog] [info] Mapping rate = 42.9052%
slurm-313513_5.out:[2022-12-15 09:10:23.450] [jointLog] [info] Mapping rate = 44.8319%
slurm-313513_6.out:[2022-12-15 09:10:25.567] [jointLog] [info] Mapping rate = 51.0876%
slurm-313513_7.out:[2022-12-15 09:11:01.478] [jointLog] [info] Mapping rate = 35.1606%
final.outslurm-313513_8.out:[2022-12-15 09:10:56.052] [jointLog] [info] Mapping rate = 46.0375%
slurm-313513_9.out:[2022-12-15 09:10:58.712] [jointLog] [info] Mapping rate = 43.7757%
[khubbar7@login1 salmon]$ grep 'Uniquely mapped reads %' ../star_2/*Log.final.out
../star_2/Badami_LB_400CH_Clone14.fastqLog.final.out:                        Uniquely mapped reads % |  0.00%
../star_2/Badami_LB_400CH_Clone15.fastqLog.final.out:                        Uniquely mapped reads % |  0.00%
../star_2/Badami_LB_400CH_Clone16.fastqLog.final.out:                        Uniquely mapped reads % |  0.00%
../star_2/Badami_LB_800CH_Clone14.fastqLog.final.out:                        Uniquely mapped reads % |  0.00%
../star_2/Badami_LB_800CH_Clone15.fastqLog.final.out:                        Uniquely mapped reads % |  0.00%
../star_2/Badami_LB_800CH_Clone16.fastqLog.final.out:                        Uniquely mapped reads % |  0.00%
../star_2/Bergeron_LB_400CH_Clone10.fastqLog.final.out:                        Uniquely mapped reads % |        0.00%
../star_2/Bergeron_LB_400CH_Clone8.fastqLog.final.out:                        Uniquely mapped reads % | 0.00%
../star_2/Bergeron_LB_400CH_Clone9.fastqLog.final.out:                        Uniquely mapped reads % | 0.00%
../star_2/Bergeron_LB_800CH_Clone10.fastqLog.final.out:                        Uniquely mapped reads % |        0.00%
../star_2/Bergeron_LB_800CH_Clone8.fastqLog.final.out:                        Uniquely mapped reads % | 0.00%
../star_2/Bergeron_LB_800CH_Clone9.fastqLog.final.out:                        Uniquely mapped reads % | 0.00%


So I obiviously dont think thats accurate BUT just looking at the total mapped reads and what the program threw out I would say STAR is better. 




