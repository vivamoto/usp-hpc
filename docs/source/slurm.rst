Slurm
=====

Slurm Workload Manager is an open source, fault-tolerant, and highly scalable cluster management and job scheduling system for large and small Linux clusters. It is used by many of the world's supercomputers and computer clusters.

Slurm manages the amount of resources allocated to each job. The number of nodes, CPU cores, memory, GPUs and period are examples of resources one can allocate to a particular job. This is ideal for distributed computing among several nodes.

Machine learning and deep learning models can be trained in HPC with Tensorflow, PyTorch, Dask or other distributed computing library.

Read the official `Quick Start User Guide <https://slurm.schedmd.com/quickstart.html>`_ for an overview of the architecture, commands and examples.

Princeton Research Computing also provides a good `introduction <https://researchcomputing.princeton.edu/support/knowledge-base/slurm>`_ to Slurm.


Commands
--------

This introductory video shows some useful commands.

..  youtube:: U42qlYkzP9k

Here's a list of some commonly used user commands. See Slurm `man pages <https://slurm.schedmd.com/man_index.html>`_ for a complete list of commands or download the  :download:`command summary PDF<_static/summary.pdf>`. Note that all Slurm commands start with **'s'**.

+-----------------------+----------------------------------------------------+
| Command               | Description                                        |
+=======================+====================================================+
| sbatch <slurm_script> | Submit a job script for later execution.           |
+-----------------------+----------------------------------------------------+
| scancel <jobid>       | Cancel a pending or running job or job step        |
+-----------------------+----------------------------------------------------+
| srun                  | Parallel job launcher (Slurm analog of mpirun)     |
+-----------------------+----------------------------------------------------+
| squeue                | Show all jobs in the queue                         |
+-----------------------+----------------------------------------------------+
| squeue -u <username>  | Show jobs in the queue for a specific user         |
+-----------------------+----------------------------------------------------+
| squeue --start        | Report the expected start time for pending jobs    |
+-----------------------+----------------------------------------------------+
| squeue -j <jobid>     | Show the nodes allocated to a running job          |
+-----------------------+----------------------------------------------------+
| scontrol show config  | View default parameter settings                    |
+-----------------------+----------------------------------------------------+
| sinfo                 | Show cluster status                                |
+-----------------------+----------------------------------------------------+


Job schedule
------------

Submit a script to the queue with ``sbatch <script>``::

	$ sbatch script.sh

	
The options of ``sbatch`` command may be inserted into the script following the ``#SBATCH`` directive::

	$ cat script.sh
	#!/bin/bash -v
	#SBATCH --partition=GPUSP4      # partition name. lince = 'GPUSP4', aguia = 'SP2'
	#SBATCH --job-name=tr-ae        # job name
	#SBATCH --nodes=1               # number of nodes allocated for this job
	#SBATCH --ntasks=2              # total number of tasks / mpi processes
	#SBATCH --cpus-per-task=8       # number OpenMP Threads per process
	#SBATCH --time=08:00:00         # total run time limit ([[D]D-]HH:MM:SS)
	#SBATCH --gres=gpu:tesla:2      # number of GPUs
	# Get email notification when job begins, finishes or fails
	#SBATCH --mail-type=ALL         # type of notification: BEGIN, END, FAIL, ALL
	#SBATCH --mail-user=your@mail   # e-mail address

	# OpenMP settings used for parallel processing. 
	# Check your library documentation for custom configuration (Tensorflow, PyTorch, Dask, etc)
	# Reference: https://www.openmp.org/spec-html/5.0/openmpch6.html#openmpse50.html
	export OMP_NUM_THREADS=1
	export MKL_NUM_THREADS=1
	export OMP_PLACES=threads
	export OMP_PROC_BIND=spread

	# Slurm controller sets these variables in the environment of the batch script
	# More variables at https://slurm.schedmd.com/sbatch.html#lbAK
	echo $SLURM_JOB_ID              # ID of job allocation
	echo $SLURM_SUBMIT_DIR          # The directory from which sbatch was invoked
	echo $SLURM_JOB_NODELIST        # List of nodes allocated to the job
	echo $SLURM_NTASKS              # Total number of cores for job

	# Load modules. Use "module avail" to list available modules.
	# This example loads a custom module.
	module use --append /scratch/11568881/modulefiles/
	module load Miniconda/1.0

	# Run the application.
	echo [`date '+%Y-%m-%d %H:%M:%S'`] Running $AE_ARCH
	srun <train_model.py>

`UiT The Arctic University of Norway <https://hpc-uit.readthedocs.io/en/latest/jobs/examples.html>`_ provides additional job script examples.





