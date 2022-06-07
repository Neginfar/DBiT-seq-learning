# Learning the environment of terminal
```
perl xxxxxx
```
current pathway

```
pwd
```

change directory to project 

```
cd project
```

making directories , creat a file with the name of the project

```
mkdir #name of files#
```
making files in the new project folder

```
cd #sample_name_file#
```
making files

```
mkdir 00.bin
```
```
mkdir 00.sh
```
```
mkdir 00.software
```
```
mkdir 01.rawdata
```
```
mkdir 00.0utput
```


need to increase the memory 

```
srun --pty -p interactive --mem=8g bash
```

looking into the folders

```
ls -lrt
```
List the names of the files in directory along with the permissions, date, time and size
```
ll
```
removing files

```
rm -rf #name_of_file/
```
remove anything that ends with sh and copy to 00.sh folder
```
mv *.sh 00.sh/
```
move folder to another folder
```
mv #folder_want_to_move/ ../#destination_folder/
```
Rreading a file

```
less -S name_of_file 
```
```
cat #file_name
```

copy files to folder 

```
cp #name_of_file #destination_folder 
```


downloading the raw data from Novogene 
```
wget #link of R1 and R2#
```
upload the sh in your directory

open a file and edit 
```
vim #name_of_file
```
type i to edit

hit esc

to save and exit
```
 :wq!
```

to submit pl if it has #
make a pl

```
vi barcode-check.pl
```
```
Set paste
```
Type i

Command v
```
:wq
```
next is chmod, change mode
```
chmod +x  #barcode-check.pl
```

and submit the pl
```
perl -c  barcode-check.p
```
giving the indirwctory and outdirectory
```
nohup perl barcode-check.pl -indir=/gpfs/ysm/project/fan/nf289/01.FFhLiver/01.rawdata -outdir=/gpfs/ysm/project/fan/nf289/01.FFhLiver/01.rawdata/PFA/test -insertsize=PFA & 
```
```
cat nohup.out
```
going to someone else directory 
```
cd /gpfs/ysm/project/fan/sb2723/00.database/
```
```
nohup perl barcode-check.pl -indir=/gpfs/ysm/project/fan/nf289/01.FFhLiver/01.rawdata -outdir=/gpfs/ysm/project/fan/nf289/01.FFhLiver/01.rawdata/PFA/test -insertsize=PFA  (wd: /gpfs/ysm/project/fan/nf289/01.FFhLiver/01.rawdata/PFA)
(wd now: /gpfs/ysm/project/fan/nf289/01.FFhLiver/01.rawdata/PFA/test/PFA)
```
looking at the running data
```
top -u nf289
```

submit a script and job
```
sbatch effective.sh 
```
to check if the job is running

```
squeue -u nf289 
```
# wokflow with ST data

downloading the sequence data from Novogene, from the link
```
wget #link of the files
```
move to the right directory 

looking into the report file
```
dispaly Rawdata_Readme.pdf 
```
for healthy human fresh sample the Q20 and Q30 should be above 90% and the percentage of CG should be 43-53%

# making shortcuts

open the bashrc
```
vi ~/.bashrc
```

type in the script and add your directory
```
alias work='cd /gpfs/ysm/project/fan/nf289'

```


going back to the directory
```
work
```
install ST-pipeline
```
module load miniconda  #only this line is used for the next times
conda create -n st-pipeline python=3.7
conda activate st-pipeline. #only this line is used for the next times
conda install -c bioconda star
conda install -c bioconda samtools openssl=1.0
st_pipeline_run.py -v

conda install PySam
conda install Numpy
conda install Cython
pip install taggd
pip install stpipeline

```
```
wget https://cpan.metacpan.org/authors/id/N/NW/NWCLARK/PerlIO-gzip-0.20.tar.gz
module avail Perl
module load Perl/5.28.0-GCCcore-7.3.0 (when you need to run PERL script, need load this module first)
tar -zxvf PerlIO-gzip-0.20.tar.gz 
cd PerlIO-gzip-0.20/
perl Makefile.PL PREFIX=/gpfs/home/nf289/project/01.FFhLiver/00.software
make
make install
```
upload 1.effective pl in 00.bin on cyberduck


submiting the effective.sh to capture barcodes A and B and ready for ST pipeline
```
sbatch effective.sh
```
going to to run R on farnam
```
https://ood-farnam.hpc.yale.edu/
```
runing the packages script from https://github.com/grmsu/DBiT-start
https://github.com/grmsu/DBiT-start.git


# looking quick into Raw Data Quality 
```
cd 01.FFhLiver/
cd 01.rawdata/
cd FF
```

get the barcode
```
zcat FF_2.fq.gz | grep AACGTGAT
```


# submiting ST pipeline
```
cd 00.sh
```
# going to ST environment
```
conda activate st-pipeline
module load miniconda
````
```
sbatch FF.stpipeline.sh
sbatch PFA.stpipeline.sh
```

looking into output 
```
cd 03.stpipeline
```
changing the geneIDs to gene names
```
sh PFA.changid.sh 
```
note: the ones that still have IDs are the pixels with no barcodes and the matrix is the number of UMIs ( genes=columns, rows=pixels)

# Image processing Matlab and PS

matlab script is found on https://github.com/edicliuyang/DBiT-seq_FFPE/tree/master/Figure_Processing

upload the images with the correct naming in 03.spatial folder using cyberduck

on R farnam,  run the file in 00.bin, named PFA_run_spatial.rmd.R ehich has been edited on terminal

# making report

```
cd 00.bin/
```
```
vi Spatial_Report.Rmd
```

(**How to install seurat)
```
#git clone https://github.com/satijalab/seurat.git
#7543 line in visualization.R + (shape = 22,)
#tar -cvf seurat.tar.gz seura
#install.packages("/gpfs/ysm/project/fan/nf289/01.FFhLiver/00.software/seurat.tar.gz", repos = NULL, type = "source")
```

```
cd ../
cd R
cd x86_64-pc-linux-gnu-library/
work
cd 01.FFhLiver/
cd 00.bin/
cat PFA_run_spatial.rmd.R 
 R
 R CMD BATCH PFA_run_spatial.rmd.R
 less PFA_run_spatial.rmd.Rout
 module avail rstudio
 module load RStudio/1.4.1717
 cat PFA_run_spatial.rmd.Rout
 which pandoc
 which rstuio
 module load RStudio/1.4.1717
 module list
 rm PFA_run_spatial.rmd.Rout
 Rscript PFA_run_spatial.rmd.R
 which rstudio
 which pandoc
 which rstudio
 export PATH=/ysm-gpfs/apps/software/RStudio/1.4.1717/bin/pandoc:$PATH
 which pandoc
 Rscript PFA_run_spatial.rmd.R
 ll PFA_Spatial_Report.Rmd
 vi PFA_run_spatial.rmd.R 
 Rscript PFA_run_spatial.rmd.R
 vi PFA_Spatial_Report.Rmd 
 Rscript PFA_run_spatial.rmd.R
 srun --pty -p interactive --mem=50g bash
 exit
 work
 cd ../sb2723/
 cd 01.Spatial_hCortex/
 cd 03.stpipeline/
  cd hC2
  cd ../../
  cd 00.bin/
  cat hC2_run_spatial.rmd.R
  ```
  # New batch of data
  running effective.sh
  
```
cd 01.FFhLiver/
cp effective. Sh  effective.new.sh
vi effective.sh 
sbatch effective.new.sh 
squeue -u nf289
cd 02.effective
```
Looking into the ligation of barcode A and B 

,,,
cd 02.effective
cd #Samplename
cat log
,,,
<images.githubusercontent.com/51792737/171759300-575fb99c-be66-4981-b1e1-9ad19173f7ac.png">

use excell with QC of data report
<img width="1227" alt="Screen Shot 2022-06-02 at 7 59 15 PM" src="https://user-images.githubusercontent.com/51792737/171759436-39e7b416-315c-418e-b9ef-fe8971d62d46.png">


 Runing ST_pipeline
 
```
srun --pty -p interactive --mem 20g bash
module load miniconda<img width="923" alt="Screen Shot 2022-06-02 at 7 57 37 PM" src="https://user-
conda activate st-pipeline

cp FF.stpipeline.sh FFHL2.stpipeline.sh 
sbatch FFHL2.stpipeline.sh 
cd 03.stpipeline
cd FFHL2
cat SAMPLE_log.txt
```
good reads should be like below
<img width="1311" alt="Screen Shot 2022-06-02 at 8 03 00 PM" src="https://user-images.githubusercontent.com/51792737/171759713-4977d8c5-5e21-44be-a514-619c0ef686c0.png">

Chande ID
```
farnam2:nf289 ~$module load miniconda
farnam2:nf289 ~$conda activate st-pipeline
Sbatch change.id.sh
```

looking into the reads with help of  https://blast.ncbi.nlm.nih.gov/Blast.cgi

```
farnam1:nf289 /gpfs/ysm/project/fan/nf289/01.FFhLiver/00.bin$ll

farnam1:nf289 /gpfs/ysm/project/fan/nf289/01.FFhLiver/00.bin$module avail perl

farnam1:nf289 /gpfs/ysm/project/fan/nf289/01.FFhLiver/00.bin$module load Perl/5.28.0-GCCcore-7.3.0

farnam1:nf289 /gpfs/ysm/project/fan/nf289/01.FFhLiver/00.bin$perl ../00.bin/fq2fa.pl FFHL5/FFHL5_CKDL220011580-1a_HVV77DSX3_L1_1.fq.gz test.fa


#R1= data, R2=barcodes

#Copy reads in NIH blast website
```


In cyberduck , upload all images in the sample folder under 03.stpipeline

Find Rmd files under 00.bin

To look into raw data
```
(st-pipeline)farnam2:nf289 /gpfs/ysm/project/fan/nf289/01.FFhLiver/01.rawdata/FFHL5$zcat FFHL5_CKDL220011580-1a_HVV77DSX3_L1_2.fq.gz | grep AACGTGATAACGT
```



Under 00.bin

```
Head -200 FFHL2.R2.fa
```


changing y axis of the image and umi heatmap
```
perl /gpfs/ysm/project/fan/nf289/01.FFhLiver/00.bin/change-xy.pl FFHL2_stdata.updated.tsv > FFHL2_stdata.updated.changexy.tsv
```
 CITE-seq in Ruddle , Yang files
 #everything for CITE-seq is the same until st-pipeline
 
 ``
 cd  /gpfs/ycga/project/fan/yl2224/processed_data
 cd /gpfs/ycga/project/fan/yl2224/processed_data/Hiplex/11122021_GBM/G3/
 ``
 
 List of 154 human proteins
 ```
 /gpfs/ycga/project/fan/yl2224/processed_data/Hiplex/11122021_GBM/G3 or G2
 ```
 Under G3
 ```
pip install biopython
farnam2:nf289 ~$module load miniconda
farnam2:nf289 ~$conda activate st-pipeline
pip install CITE-seq-Count --upgrade


fastq_process.py.( needs to modify )
&
CITE-seq-Counts.sh

Sbatch CITE-seq-Counts.sh
```
output:

<img width="954" alt="Screen Shot 2022-06-07 at 10 53 34 AM" src="https://user-images.githubusercontent.com/51792737/172412119-2469c6a6-cfab-4669-9eab-839cb53c1621.png">


flow the link below with the output of UMI_count

https://github.com/edicliuyang/Hiplex_proteome/tree/main/Data_visualization/Protein

