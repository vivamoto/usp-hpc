Working with modules
====================

Modules are a convenient way to manage environment variables for applications use. Unless you use the default installation of Anaconda available in HPC, you'll need to create custom modules. This section briefly explains how to work with modules and provides a custom module for Miniconda. See the references [#ref]_ to learn more about modules.

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

Notice that default modules have a ``(D)`` besides the name and loaded modules come with a ``(L)``. You can load a module with ``module load``::

	$ module load Anaconda/3-2019.03

Clean all loaded modules with ``module purge``::

	$ module purge

Run ``module show`` to list the commands executed in the module::

	$ module show Anaconda/3-2020.11
	----------------------------------------------------------------------------------------------------------------------------------
	   /opt/ohpc/pub/modulefiles/Anaconda/3-2020.11.lua:
	----------------------------------------------------------------------------------------------------------------------------------
	help([[This module loads /scratch/apps/gnu/anaconda3
	]])
	conflict("Anaconda","Anaconda3","anaconda","python")
	setenv("INSTALL_DIR","/scratch/apps/gnu/anaconda3")
	prepend_path("LD_LIBRARY_PATH","/scratch/apps/gnu/anaconda3")
	prepend_path("LD_LIBRARY_PATH","/scratch/apps/gnu/anaconda3/libexec")
	prepend_path("INCLUDE","/scratch/apps/gnu/anaconda3/include")
	prepend_path("PATH","/scratch/apps/gnu/anaconda3/sbin")
	prepend_path("PATH","/scratch/apps/gnu/anaconda3/bin")

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

	module use --append /scratch/<USER_ID>/modulefiles/

The next time you log in you will be able to run **module avail** or **module load** on the new module.

You also need to add these lines in your SLURM schedule script to load the environment variables::

	module use --append /scratch/<USER_ID>/modulefiles/
	module load Miniconda/1.0


Module usage
------------

Just run ``module`` to list all available options::

	$ module

	Modules based on Lua: Version 7.8.15  2019-01-16 12:46 -06:00
		by Robert McLay mclay@tacc.utexas.edu

	module [options] sub-command [args ...]

	Help sub-commands:
	------------------
	  help                              prints this message
	  help                module [...]  print help message from module(s)

	Loading/Unloading sub-commands:
	-------------------------------
	  load | add          module [...]  load module(s)
	  try-load | try-add  module [...]  Add module(s), do not complain if not found
	  del | unload        module [...]  Remove module(s), do not complain if not found
	  swap | sw | switch  m1 m2         unload m1 and load m2
	  purge                             unload all modules
	  refresh                           reload aliases from current list of modules.
	  update                            reload all currently loaded modules.

	Listing / Searching sub-commands:
	---------------------------------
	  list                              List loaded modules
	  list                s1 s2 ...     List loaded modules that match the pattern
	  avail | av                        List available modules
	  avail | av          string        List available modules that contain "string".
	  spider                            List all possible modules
	  spider              module        List all possible version of that module file
	  spider              string        List all module that contain the "string".
	  spider              name/version  Detailed information about that version of the module.
	  whatis              module        Print whatis information about module
	  keyword | key       string        Search all name and whatis that contain "string".

	Searching with Lmod:
	--------------------
	  All searching (spider, list, avail, keyword) support regular expressions:


	  -r spider           '^p'          Finds all the modules that start with `p' or `P'
	  -r spider           mpi           Finds all modules that have "mpi" in their name.
	  -r spider           'mpi$         Finds all modules that end with "mpi" in their name.

	Handling a collection of modules:
	--------------------------------
	  save | s                          Save the current list of modules to a user defined "default" collection.
	  save | s            name          Save the current list of modules to "name" collection.
	  reset                             The same as "restore system"
	  restore | r                       Restore modules from the user's "default" or system default.
	  restore | r         name          Restore modules from "name" collection.
	  restore             system        Restore module state to system defaults.
	  savelist                          List of saved collections.
	  describe | mcc      name          Describe the contents of a module collection.
	  disable             name          Disable a collection.

	Deprecated commands:
	--------------------
	  getdefault          [name]        load name collection of modules or user's "default" if no name given.
										===> Use "restore" instead <====
	  setdefault          [name]        Save current list of modules to name if given, otherwise save as the default list for you the
										user.
										===> Use "save" instead. <====

	Miscellaneous sub-commands:
	---------------------------
	  is-loaded           modulefile    return true if module is loaded
	  is-avail            modulefile    return true if module can be loaded
	  show                modulefile    show the commands in the module file.
	  use [-a]            path          Prepend or Append path to MODULEPATH.
	  unuse               path          remove path from MODULEPATH.
	  tablelist                         output list of active modules as a lua table.

	Important Environment Variables:
	--------------------------------
	  LMOD_COLORIZE                     If defined to be "YES" then Lmod prints properties and warning in color.

		--------------------------------------------------------------------------------------------------------------------------------

	Lmod Web Sites

	  Documentation:    http://lmod.readthedocs.org
	  Github:           https://github.com/TACC/Lmod
	  Sourceforge:      https://lmod.sf.net
	  TACC Homepage:    https://www.tacc.utexas.edu/research-development/tacc-projects/lmod

	  To report a bug please read http://lmod.readthedocs.io/en/latest/075_bug_reporting.html
		--------------------------------------------------------------------------------------------------------------------------------


.. [#ref] References: 

https://researchcomputing.princeton.edu/support/knowledge-base/modules

https://researchcomputing.princeton.edu/support/knowledge-base/custom-modules
