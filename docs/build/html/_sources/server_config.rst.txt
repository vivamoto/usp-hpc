Server configuration
====================

Install Miniconda and Python libraries
--------------------------------------

Python libraries installed in HPC are outdated and you may want to use newer releases. This section shows how to install Miniconda in the user's home directory, without affecting the original installation.

Miniconda installs the most recent release of Python and ``pip`` in the user's folder. The libraries installed with ``pip`` and `` conda`` are also installed in your folder. 

All commands shall be executed in the server's Linux terminal.

Check CUDA release
^^^^^^^^^^^^^^^^^^

Before installing the libraries, you need the current CUDA release to choose the right package. Run this command::

	$ nvcc --version
	nvcc: NVIDIA (R) Cuda compiler driver
	Copyright (c) 2005-2019 NVIDIA Corporation
	Built on Wed_Apr_24_19:10:27_PDT_2019
	Cuda compilation tools, release 10.1, V10.1.168

The output shows the current release is 10.1.

	
Install Miniconda
^^^^^^^^^^^^^^^^^

Miniconda is a package management system for Python and provides ``pip``, ``conda`` and the most recent Python release with basic libraries. Miniconda requires less disk space than Anaconda and is faster to install. After installing Miniconda, you may install just the libraries that you'll use. Anaconda installs many packages and applications that won't be used.

Select a Miniconda release with the Python version compatible with the libraries that you need, since not all libraries are compatible with the newest release of Python. For example, the latest release of OpenCV doesn't work with the latest release of Python.

Access Miniconda `website <https://docs.conda.io/en/latest/miniconda.html#linux-installers>`_ and copy the link with the installation script that you need. This example uses release 3.8.

.. image:: images/miniconda1.png

Use the ``wget`` command to download the script with the link you copied from Miniconda site::

	$ wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
	$ bash Miniconda3-latest-Linux-x86_64.sh
	# Update environment variables
	$ source ~/.bashrc

Install Python libraries::

	$ conda install -c conda-forge jupyterlab notebook pandas matplotlib seaborn scikit-learn IPython cvxopt opencv

	$ pip3 install hpelm opfython
	$ pip install python-hostlist

Check if scikit-learn version is the most recent. The Miniconda release I downloaded has version 0.23, and since I need 0.24 I had to manually update::

	# Update scikit-learn
	$ conda upgrade -c conda-forge scikit-learn

Install Tensorflow and PyTorch::

	# Install Tensorflow and PyTorch
	# 1. Version without GPU to install in lince (CUDA 10.1 - Python 3.9)
	$ conda install -c anaconda tensorflow
	$ conda install -c pytorch -c=conda-forge pytorch torchvision torchaudio cudatoolkit=10.1

	# 2. Version with GPU to install in aguia
	$ conda install -c conda-forge tensorflow
	$ conda install -c pytorch -c=conda-forge pytorch torchvision torchaudio cpuonly


Test libraries installation
---------------------------

After installing the libraries, run Python and import the libraries to confirm the correct version.::

	$ cat system_info.py
	#!/scratch/<YOUR_NUSP>/miniconda3/bin/python3
	import sys
	import numpy as np
	import pandas as pd
	import matplotlib as mpl
	import seaborn as sns
	import sklearn as sk
	import tensorflow as tf
	import torch
	import h5py
	import os

	print('='*20, 'Software version', '='*20)
	print("Python:", sys.version.split('\n')[0])
	print("NumPy:", np.__version__)
	print("Pandas:", pd.__version__)
	print('Matplotlib:', mpl.__version__)
	print('Seaborn:', sns.__version__)
	print("Sklearn:", sk.__version__)
	print("Tensorflow:", tf.__version__)
	print("Pytorch:", torch.__version__)
	print("h5py:", h5py.__version__)


Script output::

	$ python system_info.py
	==================== Software version ====================
	Python: 3.8.8 | packaged by conda-forge | (default, Feb 20 2021, 16:22:27)
	NumPy: 1.19.5
	Pandas: 1.0.1
	Matplotlib: 3.4.1
	Seaborn: 0.11.1
	Sklearn: 0.24.1
	Tensorflow: 2.4.0
	Pytorch: 1.4.0
	h5py: 2.10.0



System information
------------------

You may need the hardware information to choose the right software release. The following commands show the main hardware devices and the Linux release. The commands may be executed directly in the Linux terminal, or you may save in a script and run in SLURM job. Note that PyTorch provides a custom version for each CUDA version::

	$ cat system_info.sh
	#!/usr/bin/bash
	echo ========================
	echo SLURM: ID of job allocation
	echo ========================
	echo $SLURM_JOB_ID              # ID of job allocation

	echo ========================
	echo SLURM: Directory job where was submitted
	echo ========================
	echo $SLURM_SUBMIT_DIR          # Directory job where was submitted

	echo ========================
	echo SLURM: File containing allocated hostnames
	echo ========================
	echo $SLURM_JOB_NODELIST        # File containing allocated hostnames

	echo ========================
	echo SLURM: Total number of cores for job
	echo ========================
	echo $SLURM_NTASKS              # Total number of cores for job

	echo ========================
	echo SLURM: GPU devide ID that assigned to the job to use
	echo ========================
	echo $CUDA_VISIBLE_DEVICES

	echo ========================
	echo Hostname
	echo ========================
	hostname

	echo ========================
	echo Memory Info \(GB\):
	echo ========================
	free -g

	echo ========================
	echo CPU Info:
	echo ========================
	lscpu

	echo ========================
	echo Disk space
	echo ========================
	df -h

	echo ========================
	echo GPU 1
	echo ========================
	nvidia-smi

	echo ========================
	echo GPU 2
	echo ========================
	lshw -C display

	echo ========================
	echo CUDA Version
	echo ========================
	nvcc --version

	echo ========================
	echo Linux version
	echo ========================
	cat /etc/os-release

	echo ========================
	echo PATH
	echo ========================
	echo $PATH

	echo ========================
	echo Python
	echo ========================
	which python
	which python3

	echo ========================
	echo Conda
	echo ========================
	which conda
	conda --version

	echo ========================
	echo Pip
	echo ========================
	which pip
	pip --version

	echo ========================
	echo Python Library Versions
	echo ========================
	python system_info.py



