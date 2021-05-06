Slurm
=====

Slurm Workload Manager is an open source, fault-tolerant, and highly scalable cluster management and job scheduling system for large and small Linux clusters. It is used by many of the world's supercomputers and computer clusters.

The Slurm manages the amount of resources allocated to each job. The number of nodes, CPU cores, memory, GPUs and period are examples of resources one can allocate to a particular job. This is ideal for distributed computing among several nodes.

Machine learning and deep learning models can be trained in HPC with Tensorflow, PyTorch, Dask or other distributed computing library.

Read the official `Quick Start User Guide <https://slurm.schedmd.com/quickstart.html>`_ for an overview of the architecture, commands and examples.

Princeton Research Computing also provides a good `introduction <https://researchcomputing.princeton.edu/support/knowledge-base/slurm>`_


Commands
--------

This introductory video shows some useful commands.

..  youtube:: U42qlYkzP9k

Here's a list of some commonly used user commands. See Slurm `man pages <https://slurm.schedmd.com/man_index.html>`_ for a complete list of commands or download the  `command summary <https://slurm.schedmd.com/pdfs/summary.pdf>`_ PDF. Note that all Slurm commands start with **s**.

**sbatch** is used to submit a job script for later execution. The script will typically contain one or more srun commands to launch parallel tasks.

**scancel** is used to cancel a pending or running job or job step. It can also be used to send an arbitrary signal to all processes associated with a running job or job step.

**scontrol** is the administrative tool used to view and/or modify Slurm state. Note that many scontrol commands can only be executed as user root.

**squeue** reports the state of jobs or job steps. It has a wide variety of filtering, sorting, and formatting options. By default, it reports the running jobs in priority order and then the pending jobs in priority order.

**srun** is used to submit a job for execution or initiate job steps in real time. **srun** has a wide variety of options to specify resource requirements, including: minimum and maximum node count, processor count, specific nodes to use or not use, and specific node characteristics (so much memory, disk space, certain required features, etc.). A job can contain multiple job steps executing sequentially or in parallel on independent or shared resources within the job's node allocation.




Job schedule
------------
Make sure your script saves trained models, plots and result tables and prints progress status during run time::


	#!/bin/bash -v
	#SBATCH --partition=GPUSP4
	#SBATCH --ntasks=2              # number of tasks / mpi processes
	#SBATCH --cpus-per-task=8       # Number OpenMP Threads per process
	#SBATCH --job-name=tr-ae        # Job name: TReina AutoEncoder
	#SBATCH --time=3-08:00:00       # Se voce nao especificar, o default é 8 horas. O limite é 80 horas.
	#SBATCH --gres=gpu:tesla:2      # para solicitar uma GPU
	# Get email notification when job finishes or fails
	#SBATCH --mail-type=ALL         # notifications for job done & fail
	#SBATCH --mail-user=vivamoto@yahoo.com.br

	#OpenMP settings:
	export OMP_NUM_THREADS=1
	export MKL_NUM_THREADS=1
	export OMP_PLACES=threads
	export OMP_PROC_BIND=spread

	# OpenMP settings for Tensorflow:
	# we set OMP_NUM_THREADS to the number cpu cores per MPI task
	#export OMP_NUM_THREADS=${SLURM_CPUS_PER_TASK}  # Number of physical cores
	#export KMP_AFFINITY=granularity=fine,compact,1,0
	#export KMP_BLOCKTIME=0         # (or 1)
	#export KMP_SETTINGS=TRUE
	#export TF_XLA_FLAGS=--tf_xla_enable_xla_devices

	echo $SLURM_JOB_ID              # ID of job allocation
	echo $SLURM_SUBMIT_DIR          # Directory job where was submitted
	echo $SLURM_JOB_NODELIST        # File containing allocated hostnames
	echo $SLURM_NTASKS              # Total number of cores for job

	#Carregar modulos necessarios ("module avail" listará o que está disponível para o compilador selecionado)
	module use --append /scratch/11568881/modulefiles/
	module load Miniconda/1.0
	#module load Anaconda/3-2019.03 cuda/10.1

	# Autoencoder settings
	export DEBUG_MODE=FALSE
	export PROJETO=/scratch/11568881/projeto/
	export LOG=/scratch/11568881/projeto/log/
	export PYTHON=/scratch/11568881/miniconda3/bin/

	# System info
	bash $PROJETO/system_info.sh >> $LOG/system_info_$SLURM_JOB_NODELIST\.log
	srun bash $PROJETO/system_info.sh >> $LOG/system_info_srun_$SLURM_JOB_NODELIST\.log

	#run the application:
	export AE_ARCH=1_2_4_6_8_10_12  # Number of filters in each layer
	export AE_KERNEL=5_5_3_3_3_3_3  # Filter size
	echo [`date '+%Y-%m-%d %H:%M:%S'`] Running $AE_ARCH
	srun $PYTHON/python3 $PROJETO/autoencoder.py  >>  $LOG/autoencoder_$AE_ARCH\.log  2>&1

	export AE_ARCH=1_2_4_6_8_10     # Number of filters in each layer
	export AE_KERNEL=5_5_3_3_3_3    # Filter size
	echo [`date '+%Y-%m-%d %H:%M:%S'`] Running $AE_ARCH
	srun $PYTHON/python3 $PROJETO/autoencoder.py  >>  $LOG/autoencoder_$AE_ARCH\.log  2>&1

	export AE_ARCH=1_2_4_6_8        # Number of filters in each layer
	export AE_KERNEL=5_5_3_3_3      # Filter size
	echo [`date '+%Y-%m-%d %H:%M:%S'`] Running $AE_ARCH
	srun $PYTHON/python3 $PROJETO/autoencoder.py  >>  $LOG/autoencoder_$AE_ARCH\.log  2>&1

	export AE_ARCH=1_2_4_6          # Number of filters in each layer
	export AE_KERNEL=5_5_3_3        # Filter size
	echo [`date '+%Y-%m-%d %H:%M:%S'`] Running $AE_ARCH
	srun $PYTHON/python3 $PROJETO/autoencoder.py  >>  $LOG/autoencoder_$AE_ARCH\.log  2>&1

	export AE_ARCH=1_2_4            # Number of filters in each layer
	export AE_KERNEL=5_5_3          # Filter size
	echo [`date '+%Y-%m-%d %H:%M:%S'`] Running $AE_ARCH
	srun $PYTHON/python3 $PROJETO/autoencoder.py  >>  $LOG/autoencoder_$AE_ARCH\.log  2>&1

See more examples in `HPC-UiT documentation <https://hpc-uit.readthedocs.io/en/latest/jobs/examples.html>`_.




