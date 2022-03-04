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
module load Perl/5.28.0-GCCcore-7.3.0 (***when you need to run PERL script, need load this module first***)
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
  
  
  
