# Running a slurm job

Different from running commands or code on your own computer, we use slurm on the cluster. This provides a framework for starting, executing, and monitoring work and allows precise control over allocated resources.

## Useful commands

Before starting with an acutal job, here is an overview of the most useful / common commands you might use. You can also refer to the summary [here](https://slurm.schedmd.com/pdfs/summary.pdf). Further info about some of these commands is given in the sections below.

| Command | Explanation |
|----------|----------|
| squeue    | View information about jobs. |
| sbatch    | Submit a batch script for later execution.   |
| srun    | Obtain a job allocation (as needed) and execute an application.   |
| scancel    | Cancel a running or pending job.   |
| sinfo | View information about nodes and partitions.

## Running a single job

To run a single job you will use the `sbatch` command in combination with a shell script. Besides the commands or code you want to execute, you can set different parameters regarding the job to be run. While it is possible to do this in the command line using `sbatch`, it is advised to specify them in your shell script. 

The default shell script looks like:
```
#!/bin/bash
#SBATCH --job-name=my_job_name
#SBATCH --nodes=1
#SBATCH --cpus-per-task=4
#SBATCH --mem=24G

# Execute commands and/or code
```
The first line tells the shell how to interpret your program. You do not need to change this. 
Afterwards you specify your job parameters. The most important ones are given in the default file. 
- `job-name` speficies the name of your job. 
- `nodes`specifies the number of nodes to use.
- `cpus-per-task` specifies how many CPUs you need.
- `mem` specifies the required memory per node. #TODO: If one node normally runs only one task, can we set this to the max memory per node? What does it mean if this variable is set to unlimited?

There are more parameters you can use for a job if you require them. For this see [here](https://slurm.schedmd.com/pdfs/summary.pdf).

Besides the job parameters, you can specify the command and/or code you want to run. Assume you have a `train.sh` script that looks like:
```
#!/bin/bash
#SBATCH --job-name=dafirmbadl_cluster_test
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=1
#SBATCH --cpus-per-task=4
#SBATCH --mem=24G

# Change to working directory
cd /home/dafirmbadl/CD-UC-Control-Differentiation/deepLearningPipeline/

# Run train scrupt
python train.py
```

This script changes the working directory to where the python code is located. Afterwards it starts the python script `train.py`.

To actually start the job run the command:
```
sbatch train.sh
```

Now you can check whether your job is startet or still pending for resources using the `squeue` command.

## Cancelling a job

To cancel a job you first need to be able to identify the job. For this use the `squeue` command to list all jobs. Locate the job you want to cancel and note down or remember its job id. Assume the job id is 73, to cancel the job run `scancel 73`.

