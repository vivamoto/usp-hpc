Tensorflow configuration
========================

Reduce training time with proper Tensorflow and system configuration. This section covers the following topics:

#. Install Tensorflow optimized for performance
#. Save the model
#. Data format
#. OpenMP parameters
#. CPU optimization
#. GPU optimization
#. Compiler optimization
#. Distributed computing

Tensorflow installation
-----------------------

Intel created a Tensorflow version optimized for CPU. Use this installation if you won't use GPU::

	$ conda install tensorflow -c intel
	$ pip install intel-tensorflow==2.4.0

Read full instructions at Intel `website <https://software.intel.com/content/www/us/en/develop/articles/intel-optimization-for-tensorflow-installation-guide.html#Anaconda_main_linux>`_.


Save the model
--------------

It's possible to create checkpoints to save the model during training after each epoch, then resume the training from the last checkpoint. This is useful if the training time is larger than the scheduled time, or to prevent hardware failure or broken connection. Saving the model after training for later use is also possible.

Follow the instructions in this `tutorial <https://www.tensorflow.org/tutorials/keras/save_and_load>`_ to save the model.

Data format
-----------

Tensorflow stores and processes image arrays with the channel in the last dimension (channel last), also known as NHWC. The format used by Intel is `channel last <https://software.intel.com/content/www/us/en/develop/articles/intel-optimization-for-tensorflow-installation-guide.html#Anaconda_main_linux>`_, or NCHW. The meaning of each letter is:

* N: Batch size, indicating number of images in a batch.
* C: Channel, indicating number of channels in an image.
* W: Width, indicating number of pixels in horizontal dimension of an image.
* H: Height, indicating number of pixels in vertical dimension of an image.

When training on Intel CPU only, force Tensorflow to use channel first with this code::

	import tensorflow as tf
	# force channels-first ordering
	keras.backend.set_image_data_format('channels_first')

OpenMP settings
---------------

OpenMP implements parallel computing among different processors. `Intel <https://software.intel.com/content/www/us/en/develop/articles/guide-to-tensorflow-runtime-optimizations-for-cpu.html>`_ recommends the use these environment variables to configure OpenMP. For convenience, save them in your `~/.bashrc` file or setup in Python.

* OMP_NUM_THREADS
	* Maximum number of threads to use for OpenMP parallel regions if no other value is specified in the application.
	* Recommend: start with the number of physical cores/socket on the test system, and try increasing and decreasing
* KMP_BLOCKTIME
	* Time, in milliseconds, that a thread should wait, after completing the execution of a parallel region, before sleeping.
	* Recommend: start with 1 and try increasing
* KMP_AFFINITY
	* Restricts execution of certain threads to a subset of the physical processing units in a multiprocessor computer. Only valid if Hyperthreading is enabled.
	* Recommend: granularity=fine,verbose,compact,1,0
* KMP_SETTINGS
	* Enables (TRUE) or disables (FALSE) printing of OpenMP run-time library environment variables during execution
	* Recommend: Start with TRUE to ensure settings are being utilized, then use as needed

Python example::

	import os
	os.environ["OMP_NUM_THREADS"] = "8"  # Number of physical cores
	os.environ["KMP_AFFINITY"] = "granularity=fine,compact,1,0"
	os.environ["KMP_BLOCKTIME"] = "0"  #(or 1)
	os.environ["KMP_SETTINGS"] = "TRUE"

CPU optimization
----------------

Set the number of CPU cores that Tensorflow can use with these parameters:

* intra_op_parallelism_threads
	* Number of threads used within an individual op for parallelism
	* Recommend: start with the number of cores/socket on the test system, and try increasing and decreasing
* inter_op_parallelism_threads
	* Number of threads used for parallelism between independent operations.
	* Recommend: start with the number of physical cores on the test system, and try increasing and decreasing
* device_count
	* Maximum number of devices (CPUs in this case) to use
	* Recommend: start with the number of cores/socket on the test system, and try increasing and decreasing
* allow_soft_placement
	* Set to True/enabled to facilitate operations to be placed on CPU instead of GPU

Example::

	import tensorflow as tf
	tf.config.threading.set_inter_op_parallelism_threads(8)  # Use 8 physical cores
	tf.config.threading.set_intra_op_parallelism_threads(8)  # Use 8 physical cores
	tf.config.set_soft_device_placement(True)

Reference: https://software.intel.com/content/www/us/en/develop/articles/guide-to-tensorflow-runtime-optimizations-for-cpu.html

GPU optimization
----------------

Insert this code in Google Colab to make sure GPU is enabled::

	import tensorflow as tf
	# Show available devices: CPU and GPU
	print(tf.config.list_physical_devices())

	# Check that we are using a GPU, if not switch runtimes
	#   using Runtime > Change Runtime Type > GPU
	assert len(tf.config.list_physical_devices('GPU')) > 0


Compiler optimization
---------------------

`XLA (Accelerated Linear Algebra) <https://www.tensorflow.org/xla>`_ is a domain-specific compiler for linear algebra that can accelerate TensorFlow models with potentially no source code changes. Enable XLA in Python or save the environment variable in `` ~/.bashrc``::
 
	import os
	os.environ['TF_XLA_FLAGS'] = '--tf_xla_enable_xla_devices'


Pipeline optimization
---------------------

Data input pipeline used during training may impact performance. An efficient pipeline reads data from disk for the next batch while the GPU processes the current batch.

See how to achieve `better performance with the tf.data API <https://www.tensorflow.org/guide/data_performance>`_ to build an optimized data pipeline.

Distributed computing
---------------------

Tensorflow can distribute computing in `more than one GPU <https://www.tensorflow.org/guide/distributed_training>`_ in the same computer or in `several servers <https://www.tensorflow.org/tutorials/distribute/keras>`_.

This example shows how to enable distributed computing in one or more GPUs or use the default strategy if no GPU is found::

	import tensorflow as tf
	# Distributed training: GPU settings
	if tf.config.list_physical_devices('GPU'):
	  strategy = tf.distribute.MirroredStrategy()
	else:  # use default strategy
	  strategy = tf.distribute.get_strategy() 

Then use the **strategy** to create the model, optimizer and compile the model::

	with strategy.scope():
		optimizer = tf.keras.optimizers.SGD()
		model = tf.keras.Sequential([tf.keras.layers.Dense(1, input_shape=(1,))])
		model.compile(loss='mse', optimizer=optimizer)
		
See `distributed training with TensorFlow <https://www.tensorflow.org/guide/distributed_training>`_ for complete explanation about distributed computing.


