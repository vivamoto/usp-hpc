Working with modules
====================

Modules are a convenient way to manage environment variables for applications use. Unless you use the default installation of Anaconda available in HPC, you'll need to modify custom modules. This section briefly explains how to work with modules and provides a custom module for Miniconda. See the references [#ref]_ to learn more about modules.

Environment modules set environment variables with specific values for each application. Run **module avail** to list all modules available::

	$ module avail

	---------------------------------------------------------------- /opt/ohpc/pub/moduledeps/gnu7-openmpi3 ----------------------------------------------------------------
	   adios/1.13.1        imb/2018.1     netcdf-cxx/4.3.0        phdf5/1.10.2        py2-scipy/1.1.0     scalasca/2.3.1    superlu_dist/5.3.0
	   boost/1.67.0 (D)    mfem/3.3.2     netcdf-fortran/4.4.4    pnetcdf/1.9.0       py3-mpi4py/3.0.0    scorep/4.0        tau/2.27.1
	   fftw/3.3.7          mpiP/3.4.1     netcdf/4.6.1            ptscotch/6.0.4      py3-scipy/1.1.0     sionlib/1.7.1     trilinos/12.12.1
	   hypre/2.14.0        mumps/5.1.2    petsc/3.9.1             py2-mpi4py/3.0.0    scalapack/2.0.2     slepc/3.9.1

	-------------------------------------------------------------------- /opt/ohpc/pub/moduledeps/gnu7 ---------------------------------------------------------------------
	   R/3.5.0    hdf5/1.10.2    mpich/3.2.1    openblas/0.2.20    openmpi3/3.1.0 (L)    py2-numpy/1.14.3    superlu/5.2.1
	   gsl/2.4    metis/5.1.0    ocr/1.0.1      openmpi/1.10.7     pdtoolkit/3.25        py3-numpy/1.14.3

	---------------------------------------------------------------------- /opt/ohpc/pub/modulefiles -----------------------------------------------------------------------
	   EasyBuild/3.8.1           clustershell/1.8    gnu7/7.3.0  (L)    intel/19.0.4.243        papi/5.6.0        singularity/3.1.0
	   autotools          (L)    cmake/3.13.4        gnu8/8.3.0         llvm5/5.0.1             pmix/2.2.2        valgrind/3.14.0
	   charliecloud/0.9.7        gnu/5.4.0           hwloc/2.0.3        ohpc             (L)    prun/1.3   (L)

	-------------------------------------------------------------------------- /apps/modulefiles ---------------------------------------------------------------------------
	   Anaconda/2-2019.03        Gromacs/5.1.4-cuda-mpi    Gromacs/2019.3-cuda (D)    amber/19-cpu        canal/1.5     lammps/7Aug19         mkl/2019.4.243
	   Anaconda/3-2019.03 (D)    Gromacs/5.1.4-cuda        Gromacs/2019.3-mpi         amber/19-gpu        curves/3.0    lammps/29Oct20 (D)    ox/8.02-0-gnu
	   Gromacs/4.0.7             Gromacs/5.1.4-mpi         NAMD/2.13-CUDA             amber/20-gpu (D)    dssp          magma/2.5.1           pgi/19.10
	   Gromacs/4.6.7             Gromacs/2018.3-cuda       amber/18                   boost/1_71_0        g_mmpbsa      megacc/10.2.5         relion/3.1

	--------------------------------------------------------------------------- /opt/modulefiles ---------------------------------------------------------------------------
	   cuda/8.0    cuda/10.0    cuda/10.1 (L,D)


	  Where:
	   D:  Default Module
	   L:  Module is loaded

	Use "module spider" to find all possible modules.
	Use "module keyword key1 key2 ..." to search for all possible modules matching any of the "keys".



You can load a module with ``module load``::

	$ module load Anaconda/3-2019.03


Create custom module
--------------------

You can create custom modules to modify the environment variables. The custom module presented here is a copy of **lince**'s ``Anaconda/3-2019`` module modified for Miniconda. 

Custom modules are saved in the ``modulefiles`` folder. The folder structure is ``modulefiles/<app_name>/<version>``, where ``<app_name>`` is the application name and ``<version>`` is the module version. Actually, it may be any name but we use this convention for simplicity.

	$ mkdir -p ~/modulefiles/Miniconda/
	
Now, use a text editor such as ``vim`` or ``nano`` to create the file ``1.0`` with the following content.::

	$ cd ~/modulefiles/Miniconda
	# use a text editor to create the module file called 1.0 (which is the version)
	$ cat 1.0
	#%Module#######################################

	set INSTALL_DIR /scratch/<YOUR_NUSP>/miniconda3

	conflict pyhton  anaconda  Anaconda miniconda Miniconda

	prepend-path PATH ${INSTALL_DIR}/bin
	prepend-path INCLUDE ${INSTALL_DIR}/include/
	prepend-path -d  " " CPPFLAGS   -I${INSTALL_DIR}/include
	prepend-path -d " " LDFLAGS -L${INSTALL_DIR}/lib
	prepend-path LD_LIBRARY_PATH ${INSTALL_DIR}/lib
	prepend-path MANPATH ${INSTALL_DIR}/share/man

Where YOUR_NUSP is your user id.

Add the module path to MODULEPATH
---------------------------------

Now that the module file has been created, one just needs to add the following line to your ``~/.bashrc`` file so that it will be found::

	module use --append /scratch/11568881/modulefiles/

The next time you log in you will be able to run **module avail** or **module load** on the new module.

You also need to add these lines in your SLURM schedule script to load the environment variables::

	module use --append /scratch/11568881/modulefiles/
	module load Miniconda/1.0


.. [#ref] References: 

https://researchcomputing.princeton.edu/support/knowledge-base/modules

https://researchcomputing.princeton.edu/support/knowledge-base/custom-modules
