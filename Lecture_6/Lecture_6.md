---
title: "Lecture 6"
author: "Alejandro Rojas"
output: 
  html_document: 
    keep_md: yes
---



## SLURM

* Job scheduler used for most HPCC's systems currently
* If you know how to use `qsub`, it will be easy to transition to SLURM.

Today's focus writing SLURM job scripts and we will also practice how to check job status.
```
| SLURM               | TORQUE                   | Function                           |
| ------------------- | ------------------------ | ---------------------------------- |
| sbatch              | qsub                     | submit <job file>                  |
| srun                | qsub -I                  | submit interactive job             |
| squeue              | qstat                    | list all queued jobs               |
| squeue -u -rfeynman | qstat -u rfeynman        | list queued jobs for user rfeynman |
| scancel             | qdel                     | cancel <job#>                      |
| sinfo               | shownodes -l -n;qstat -q | node status;list of queues         |
````

slurm	use:

`sbatch`	submit <job file>

`srun` submit interactive job

`squeue` list all queued jobs

`squeue -u rfeynman` list queued jobs for user rfeynman

`scancel`	cancel <job#>

`sinfo` node status;list of queues



## Runing an interactive session

Running an interactive session will avoid getting warnings or being flag for using resources out of the queue

Let's download these files, but let's make sure that you are in one of the nodes that have open ports (pinnacle-l[6,7,13,14]):

```
curl https://ftp.ncbi.nlm.nih.gov/geo/samples/GSM2527nnn/GSM2527046/suppl/GSM2527046_AG1GY_128nM_SAM_offset0_0_NeighborhoodMapping_R1_001.fastq.gz -o Sacc.R1.fastq.gz

curl https://ftp.ncbi.nlm.nih.gov/geo/samples/GSM2527nnn/GSM2527046/suppl/GSM2527046_AG1GY_128nM_SAM_offset0_0_NeighborhoodMapping_R2_001.fastq.gz -o Sacc.R2.fastq.gz
```

Now we can start an interactive session:
```
srun --nodes=1 --ntasks-per-node=1  --cpus-per-task=8 --partition comp06 --time=6:00:00 --pty /bin/bash
```


## Basic SLURM scripts

A basic script in slurm looks like:

```
#!/bin/bash
#SBATCH --job-name=espresso
#SBATCH --output=zzz.slurm
#SBATCH --nodes=4
#SBATCH --tasks-per-node=32
#SBATCH --time=00:00:10
#SBATCH --partition comp06
module purge
module load intel/14.0.3 mkl/14.0.3 fftw/3.3.6 impi/5.1.2
cd $SLURM_SUBMIT_DIR
cp *.in *UPF /scratch/$SLURM_JOB_ID
cd /scratch/$SLURM_JOB_ID
mpirun -ppn 16 -hostfile /scratch/${SLURM_JOB_ID}/machinefile_${SLURM_JOB_ID} -genv OMP_NUM_THREADS 4 -genv MKL_NUM_THREADS 4 /share/apps/espresso/qe-6.1-intel-mkl-impi/bin/pw.x -npools 1 <ausurf.in
mv ausurf.log *mix* *wfc* *igk* $SLURM_SUBMIT_DIR/
pinnacle-l1:$

```

Link cheat sheet: https://www.chpc.utah.edu/presentations/SlurmCheatsheet.pdf

## Writing your first SLURM script

Let's use blast as an example for writing our first script. Firt we need to make sure that
the latest blast is available

```
module spider blast
```

This is the output that we get:

```
----------------------------------------------------------------------
  blast:
----------------------------------------------------------------------
    Description:
      The Basic Local Alignment Search Tool (BLAST) finds regions of
      local similarity between sequences.

     Versions:
        blast/2.3.0+
        blast/2.4.0+
        blast/2.6.0+
        blast/2.7.1+bin
        blast/2.9.0+bin
        blast/2.9.0+src
        blast/2.9.0+
        blast/2.10.0+bin
        blast/2.10.0+src
        blast/2.10.0
        blast/2.11.0+bin
        blast/2.11.0
        blast/2.12.0+bin
        blast/2.12.0+src
        blast/2.13.0+bin
     Other possible modules matches:
        crb-blast  gcc-11.2.1/SKYLAKEX/clblast  gcc-11.2.1/SKYLAKEX/ncbi-magicblast  ...

```

In that case we want to use `blast/2.13.0+bin`, so let's start building the script 
in `script.sh` file by using nano.

```
nano script.sh
```

In nano, let's add the the following lines:

```
#!/bin/bash
#SBATCH --job-name=class_test
#SBATCH --output=ct_output.slurm
#SBATCH --nodes=4
#SBATCH --tasks-per-node=32
$SBATCH --time=00:10:00

module load blast/2.13.0+bin

makeblastdb -in <reference.fa> -dbtype nucl -parse_seqids -out <database_name> -title "Database title"
```
