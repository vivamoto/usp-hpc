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



