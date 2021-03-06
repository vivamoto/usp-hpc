.. USP HPC documentation master file, created by
   sphinx-quickstart on Sun May  2 12:59:43 2021.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Welcome to USP HPC's documentation!
===================================

High-Performance Computing (HPC) at Universidade de Sao Paulo (USP) comprises two clusters of servers for parallel and distributed computing for scientific research. Use Python libraries to train machine and deep learning models in a single server on GPU and scale up to the full cluster for large datasets. 

Common attributes:

* Workload manager (Slurm) that schedules jobs in all servers.
* Shared network file system visible to all servers.
* Limited local storage.
* Login node with no GPU in **lince** cluster.

Cluster **aguia** contains servers for CPU processing while **lince** cluster is for GPU processing. Access to the clusters through **shark**.

Both clusters allow parallel and distributed computing for scientific purposes, for example training of machine learning and deep learning models.

This documentation explains how to:

#. Setup a development environment for testing and debugging.
#. Schedule and manage Slurm jobs.
#. Use Python libraries to train machine learning and deep learning models.

This is a quick start guide for new users and may save several hours of searching and testing. Detailed and complete information on each topic is available in the Internet.


Cluster Architecture
--------------------

.. image:: images/hpc.PNG

Cluster **aguia**:

* 128 servers
* 512GB RAM
* 20 cores
* Intel(R) Xeon(R) CPU E7- 2870 @ 2.40GHz
* 256TB filesystem

Cluster **lince**:

* 32 servers
* 128GB RAM
* 16 cores
* Intel(R) Xeon(R) E5- 2680 @ 2.70GHz
* 2 GPUs NVIDIA Tesla K20m 
* 55TB filesystem


.. toctree::
   :maxdepth: 2
   :caption: Configuration 

   client_configuration 
   server_configuration
   install_gdrive

.. toctree::
   :maxdepth: 2
   :caption: Code Development

   code_development
   tensorflow_settings

.. toctree::
   :maxdepth: 2
   :caption: SLURM

   custom_module
   slurm
   gpucheck
   linux

.. toctree::
   :caption: Other Information

   resources
   support
   about
   license 

