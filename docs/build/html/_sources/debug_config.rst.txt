Code development
================

The job scheduled in HPC goes to the last position in a queue that may take several days to start execution. For this reason, HPC is not suited for testing and debugging. This sections shows how to create a test environment and schedule a job.

Test and debug
--------------

The GPU is disabled in the login node of **lince**, and this node is used mainly for job schedule. A feasible alternative is to test and debug in Google Colab or Kaggle before submitting to SLURM. 

An easy way to distinguish between debug and production modes is the use of and environment variable, for example ``DEBUG_MODE``. In debug mode, the code loads a subset of the dataset and runs for a few epochs, since the goal is just make sure the code is error free. Then, in production mode the code processes the entire dataset and full epochs. In production mode you may also want to test several model architectures. The examples in this section show how to test an autoencoder with different number of layers.

Insert this piece of code in the first cell of Google Colab. The second line enables debug mode, and the third and fourth lines are the model configuration.::

	import os
	os.environ['DEBUG_MODE'] = 'true'
	os.environ['AE_ARCH'] = '1_2_4_6_8_10_12'    # Number of filters in each layer
	os.environ['AE_KERNEL'] = '5_5_3_3_3_3_3'    # Filter size in each layer

You may want to test in VS Code before sending the script to SLURM. This can be done by setting the debug mode as default when you login, and just disable in the schedule script. 

Enable debug mode in **lince** inserting this code in your ``~/.bashrc`` file::

	export DEBUG_MODE=TRUE          #
	export AE_ARCH=1_2_4_6_8_10_12  # Number of filters in each autoencoder layer
	export AE_KERNEL=5_5_3_3_3_3_3  # Filter size in each layer

Read the debug mode in the Python script body::

	# Debug mode is lightweight components to be able to run in Google Colab
	debug_mode = os.getenv('DEBUG_MODE').title() in ['True', '1']

Set 5 epochs in debug mode and 5000 otherwise::

	num_epochs = 5 * debug_mode or 5000	
	

Google Colab setup
------------------

In the first cell of Colab, insert this code to mount GDrive in Colab::

	# Mount Google Drive
	from google.colab import drive
	drive.mount("/content/drive")

Now you have access to the scripts in **lince** saved in Drive. Example of code to copy scripts from GDrive to Colab::

	import os
	os.system('cp /content/drive/MyDrive/<FOLDER>/*.py .')
	
	
Copy trained models from folder ``model/<MODEL_NAME>`` to Drive::

	import os, glob
	
	def save_models_GDrive():
	  # Create folder to save models
	  for fdir in glob.glob('model/*'):
		gdir = '/content/drive/MyDrive/lince-colab/' + fdir.split('/')[1]
		if not os.path.exists(gdir):
		  os.system('mkdir {}'.format(gdir))

	  # Copy trained models do Google Drive
	  for model in glob.glob('model/*/*'):
		fdir = model.split('/')[1]
		fname = model.split('/')[2]
		cmd = 'cp model/{}/{} /content/drive/MyDrive/lince-colab/{}/{}'.format(fdir, fname, fdir, fname)
		print(cmd)
		os.system(cmd)


.. _job-schedule:
Job schedule
------------

Make sure your script saves trained models, plots and result tables and prints progress status during run time::


	#!/bin/bash -v
	#SBATCH --partition=GPUSP4
	#SBATCH --ntasks=2              # Maximum number of tasks / mpi processes
	#SBATCH --cpus-per-task=8       # Number OpenMP Threads per process
	#SBATCH --job-name=tr-ae        # Job name
	#SBATCH --time=3-08:00:00       # Set a limit on the total run time of the job allocation.
	#SBATCH --gres=gpu:tesla:2      # Uses 2 GPUs
	# Get email notification when job finishes or fails
	#SBATCH --mail-type=ALL         # Valid type values are NONE, BEGIN, END, FAIL, REQUEUE, ALL 
	#SBATCH --mail-user=user@usp.com #  User to receive email notification of state changes


	#OpenMP settings:
	export OMP_NUM_THREADS=1
	export MKL_NUM_THREADS=1
	export OMP_PLACES=threads
	export OMP_PROC_BIND=spread

	# TensorFlow Runtime optimizations for CPU
	# Ref: https://software.intel.com/content/www/us/en/develop/articles/guide-to-tensorflow-runtime-optimizations-for-cpu.html
	#export OMP_NUM_THREADS=${SLURM_CPUS_PER_TASK}  # Number of physical cores
	#export KMP_AFFINITY=granularity=fine,compact,1,0
	#export KMP_BLOCKTIME=0         # (or 1)
	#export KMP_SETTINGS=TRUE
	#export TF_XLA_FLAGS=--tf_xla_enable_xla_devices

	echo $SLURM_JOB_ID              # ID of job allocation
	echo $SLURM_SUBMIT_DIR          # Directory job where was submitted
	echo $SLURM_JOB_NODELIST        # File containing allocated hostnames
	echo $SLURM_NTASKS              # Total number of cores for job

	# Load custom module with Miniconda environment variables
	module use --append /scratch/11568881/modulefiles/
	module load Miniconda/1.0

	# Autoencoder settings
	export DEBUG_MODE=FALSE                     # Production mode runs entire dataset and all epochs.
	export PROJECT=/scratch/11568881/project/
	export LOG=/scratch/11568881/project/log/

	# Save system info
	bash $PROJECT/system_info.sh >> $LOG/system_info_$SLURM_JOB_NODELIST\.log

	# Run the application
	export AE_ARCH=1_2_4_6_8_10_12  # Number of filters in each layer
	export AE_KERNEL=5_5_3_3_3_3_3  # Filter size
	echo [`date '+%Y-%m-%d %H:%M:%S'`] Running $AE_ARCH
	srun python3 $PROJECT/autoencoder.py  >>  $LOG/autoencoder_$AE_ARCH\.log  2>&1

	export AE_ARCH=1_2_4_6_8_10     # Number of filters in each layer
	export AE_KERNEL=5_5_3_3_3_3    # Filter size
	echo [`date '+%Y-%m-%d %H:%M:%S'`] Running $AE_ARCH
	srun python3 $PROJECT/autoencoder.py  >>  $LOG/autoencoder_$AE_ARCH\.log  2>&1

	export AE_ARCH=1_2_4_6_8        # Number of filters in each layer
	export AE_KERNEL=5_5_3_3_3      # Filter size
	echo [`date '+%Y-%m-%d %H:%M:%S'`] Running $AE_ARCH
	srun python3 $PROJECT/autoencoder.py  >>  $LOG/autoencoder_$AE_ARCH\.log  2>&1

See more examples in `HPC-UiT documentation <https://hpc-uit.readthedocs.io/en/latest/jobs/examples.html>`_.

