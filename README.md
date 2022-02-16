# learning the environment of terminal
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





