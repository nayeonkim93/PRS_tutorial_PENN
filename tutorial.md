In this tutorial, we are going to construct polygenic risk score using PRS-CS. \
References : [PRS-CS github](https://github.com/getian107/PRScs), [PRS-CS paper](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6467998/). \
The data we are going to use are already preprocessed or downloaded.


Make sure Python and PLINK are installed on your server or local machine, either system-wide or within a Conda environment. Upload the PRS file to your working directory and unzip it before proceeding.

### 1. Navigate to the PRS folder and create a new directory called result to store outputs from the practice session
```
cd PRS
``` 
```
mkdir result 
``` 

### 2. While staying in the 3_PRS directory, run the PRS-CS to calculate polygenic risk scores.
```
python PRScs/PRScs.py \
--ref_dir=data/reference/ldblk_1kg_eas \
--bim_prefix=data/plink/sample \
--sst_file=data/summary_stat/sumstats_prscs.txt \
--n_gwas=177618 \
--out_dir=result/prscs
```
Use the nohup command to run PRS-CS in the background, allowing the process to continue even if the terminal is closed. The output will be saved to result/nohup.out.
```
nohup python PRScs/PRScs.py \
--ref_dir=data/reference/ldblk_1kg_eas \
--bim_prefix=data/plink/sample \
--sst_file=data/summary_stat/sumstats_prscs.txt \
--n_gwas=177618 \
--out_dir=result/prscs > result/nohup.out &

``` 

### 3. Navigate to the result folder and combine PRS-CS Output Files (chr1–chr22) into a Single File
```
cd result
``` 
```
for i in {1..22}; do cat "prscs_pst_eff_a1_b0.5_phiauto_chr$i.txt" >> prscs_chr1-22.txt; done
``` 

### 4.Move back to the parent directory and run PLINK to calculate the polygenic risk score (PRS) based on the output from PRS-CS.
Columns 2, 4, and 6 represent the SNP ID, the effect allele, and the effect size of the effect allele, respectively.

```
cd ..
```
```
path_to_plink/plink \
--bfile data/plink/sample \
--score result/prscs_chr1-22.txt 2 4 6 \
--out result/score
``` 
#### NOTE: Since are using relative paths in the commands (except for Step 4), please make sure to run all commands from within the PRS directory. Alternatively, you can modify the commands to use absolute paths if you’re running them from a different location.
