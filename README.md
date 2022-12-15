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
| Badami_LB_400CH_Clone14      |
| Badami_LB_400CH_Clone15      |
| Badami_LB_400CH_Clone16      |
| Badami_LB_800CH_Clone14      |
| Badami_LB_800CH_Clone15      |
| Badami_LB_800CH_Clone16      |
| Bergeron_LB_400CH_Clone10    |
| Bergeron_LB_400CH_Clone8     |
| Bergeron_LB_400CH_Clone9     |
| Bergeron_LB_800CH_Clone10    |
| Bergeron_LB_800CH_Clone8     |
| Bergeron_LB_800CH_Clone9     |

##STAR								
|    	NAME                 | Uniquely mapped reads %	| % of reads mapped to multiple loci	| % of reads mapped to too many loci	| % of reads unmapped: too many mismatches	| % of reads unmapped: too short	| % of reads unmapped: other	| % of chimeric reads |	
| ------------------------ | -----------------------	| ----------------------------------	| ----------------------------------	| ----------------------------------------	| ------------------------------	| --------------------------	| ------------------- |
Badami_LB_400CH_Clone14    |			
Badami_LB_400CH_Clone15		 |			
Badami_LB_400CH_Clone16		 |		
Badami_LB_800CH_Clone14		 |				
Badami_LB_800CH_Clone15		 |			
Badami_LB_800CH_Clone16		 |			
Bergeron_LB_400CH_Clone10	 |				
Bergeron_LB_400CH_Clone8	 |				
Bergeron_LB_400CH_Clone9	 |				
Bergeron_LB_800CH_Clone10	 |					
Bergeron_LB_800CH_Clone8	 |				
Bergeron_LB_800CH_Clone9	 |					



## Now Salmon!

