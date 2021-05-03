GPU Health Check
================

The **lince** servers have 2 NVIDIA Tesla GPUs installed. This cluster should be used to run GPU jobs; if you don't need GPU then use **aguia** instead.

You may check the GPU usage with the ``nvidia-smi`` command in any server except the login server which is used only for job scheduling.

Example of GPU using 67MiB of memory and process ID 29150::

    (base) [11568881@lince2-002:~]$ nvidia-smi
    Sun May  2 16:54:50 2021
    +-----------------------------------------------------------------------------+
    | NVIDIA-SMI 418.67       Driver Version: 418.67       CUDA Version: 10.1     |
    |-------------------------------+----------------------+----------------------+
    | GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
    | Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
    |===============================+======================+======================|
    |   0  Tesla K20m          Off  | 00000000:05:00.0 Off |                    0 |
    | N/A   68C    P0   106W / 225W |     78MiB /  4743MiB |     74%      Default |
    +-------------------------------+----------------------+----------------------+
    |   1  Tesla K20m          Off  | 00000000:83:00.0 Off |                    0 |
    | N/A   30C    P8    16W / 225W |      0MiB /  4743MiB |      0%      Default |
    +-------------------------------+----------------------+----------------------+

    +-----------------------------------------------------------------------------+
    | Processes:                                                       GPU Memory |
    |  GPU       PID   Type   Process name                             Usage      |
    |=============================================================================|
    |    0     29150      C   ...gramas/intel/gromacs-5.1.4-cuda/bin/gmx    67MiB |
    +-----------------------------------------------------------------------------+
    (base) [11568881@lince2-002:~]$


Message "No running processes found" on idle GPUs::

    (base) [11568881@lince2-001:~]$ nvidia-smi
    Sun May  2 16:55:18 2021
    +-----------------------------------------------------------------------------+
    | NVIDIA-SMI 418.67       Driver Version: 418.67       CUDA Version: 10.1     |
    |-------------------------------+----------------------+----------------------+
    | GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
    | Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
    |===============================+======================+======================|
    |   0  Tesla K20m          Off  | 00000000:05:00.0 Off |                    0 |
    | N/A   36C    P8    25W / 225W |     11MiB /  4743MiB |      0%      Default |
    +-------------------------------+----------------------+----------------------+
    |   1  Tesla K20m          Off  | 00000000:83:00.0 Off |                    0 |
    | N/A   28C    P8    16W / 225W |      0MiB /  4743MiB |      0%      Default |
    +-------------------------------+----------------------+----------------------+

    +-----------------------------------------------------------------------------+
    | Processes:                                                       GPU Memory |
    |  GPU       PID   Type   Process name                             Usage      |
    |=============================================================================|
    |  No running processes found                                                 |
    +-----------------------------------------------------------------------------+
    (base) [11568881@lince2-001:~]$


Example of error message::

    (base) [11568881@lince2-008:~]$ nvidia-smi
    Unable to determine the device handle for GPU 0000:05:00.0: GPU is lost.  Reboot the system to recover this GPU


    (base) [11568881@lince2-008:~]$


Check all servers
-----------------

This Python script connects to each server and checks the GPU status. Then, it prints a list of servers with idle GPU and GPUs with error message::

	$ cat gpustat.py
	#!/scratch/11568881/miniconda3/bin/python

	import os

	# Connect to each server and collect GPU information.
	# Result is saved in a log file.
	def gpustatus():
		if os.path.exists('gpustat.log'):
			os.system('rm gpustat.log')
		for n in range(1, 32):
			s = ("000" + str(n))[-3:]
			os.system('ssh lince2-{} \"hostname;nvidia-smi\" >> gpustat.log'.format(s))

	# Opens the log file and check the GPU usage and availability.
	# Print the list of servers with idle GPU and GPU down.
	def procfile():
		nogpu = []
		gpudown = []
		with open('gpustat.log', 'r') as f:
			rows = f.readlines()
			for row in rows:
				if 'lince2-' in row:
					server = row.split('\n')[0]
				elif "No running processes found" in row:
					nogpu.append(server)
				elif "Unable to determine the device handle for GPU" in row:
					gpudown.append(server)

		# Print results
		print("="*5, "No running processes found", "="*10)
		for item in nogpu:
			print(item)
		print("="*5,"Unable to determine the device handle", "="*10)
		for item in gpudown:
			print(item)

	# Execute GPU checks
	gpustatus()
	procfile()


Script output::

	$ python gpustat.py
	===== No running processes found ==========
	lince2-001.hpc.usp.br
	lince2-009.hpc.usp.br
	lince2-011.hpc.usp.br
	lince2-021.hpc.usp.br
	lince2-022.hpc.usp.br
	===== Unable to determine the device handle ==========
	lince2-008.hpc.usp.br
	lince2-011.hpc.usp.br
	
