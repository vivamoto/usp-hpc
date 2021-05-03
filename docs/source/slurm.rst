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



