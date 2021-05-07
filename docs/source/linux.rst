Useful Linux Commands
=====================

Manual pages
^^^^^^^^^^^^

The ``man`` command provides an online reference manual of any command. For example, use this command to see reference of ``ls``::

	$ man ls

Linux processes
^^^^^^^^^^^^^^^

The ``top`` command shows CPU and memory usage, running processes and other information. It provides a dynamic view of the sytem usage, 

 	$ top
	top - 14:32:01 up 546 days, 5 min,  6 users,  load average: 0.40, 0.33, 0.27
	Tasks: 351 total,   1 running, 331 sleeping,  17 stopped,   2 zombie
	%Cpu(s):  0.2 us,  0.2 sy,  0.0 ni, 97.0 id,  2.6 wa,  0.0 hi,  0.1 si,  0.0 st
	KiB Mem : 13182566+total,  2305124 free, 78298656 used, 51221880 buff/cache
	KiB Swap:        0 total,        0 free,        0 used. 49155016 avail Mem

	  PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND
	 6763 ganglia   20   0  406312 131972   1376 S   2.7  0.1  17495:02 gmond
	 6695 ganglia   20   0  632268  15936    396 S   1.8  0.0  21331:43 gmetad
	30101 11568881  20   0  162268   2568   1612 R   1.4  0.0   0:00.07 top
	11222 mysql     20   0   54.8g  49.1g   4480 S   0.9 39.0   1790:49 mysqld
		1 root      20   0  193924   6028   1728 S   0.0  0.0 295:43.67 systemd
		2 root      20   0       0      0      0 S   0.0  0.0   0:13.83 kthreadd
		3 root      20   0       0      0      0 S   0.0  0.0 402:24.01 ksoftirqd/0
		8 root      rt   0       0      0      0 S   0.0  0.0   0:25.18 migration/0
		9 root      20   0       0      0      0 S   0.0  0.0   0:00.00 rcu_bh
	   10 root      20   0       0      0      0 S   0.0  0.0 544:55.47 rcu_sched
	   11 root       0 -20       0      0      0 S   0.0  0.0   0:00.00 lru-add-drain
	   12 root      rt   0       0      0      0 S   0.0  0.0   3:27.55 watchdog/0
	   13 root      rt   0       0      0      0 S   0.0  0.0   3:18.02 watchdog/1
	   14 root      rt   0       0      0      0 S   0.0  0.0   1:22.39 migration/1
	   15 root      20   0       0      0      0 S   0.0  0.0 370:18.79 ksoftirqd/1

Running in background
^^^^^^^^^^^^^^^^^^^^^

Connection may drop for several reasons, including computer hibernation. When this occurs, Linux terminates all processes running in the disconnected terminal and you have to run them again. The simplest way to keep the process running is sending to background by adding ``&`` to the command::

	$ sleep 60 & 
	
A message appears when the process ends::

	[1]+  Done                    sleep 60
	
To send a running process to the background, suspend execution with ``<CTRL> + z`` and execute ``bg`` to send to background::


	$ sleep 60
	^Z
	[1]+  Stopped                 sleep 60
	$ bg
	[1]+ sleep 60 &
	
Bring back to foreground with ``fg``.

screen
^^^^^^

The ``screen`` command opens a virtual terminal that keeps running even when the main terminal is disconnected. Another real terminal can connect to it, which makes it useful when installing packages. 

Steps to use ``screen``::

#. Open a virtual terminal with the ``screen`` command.
#. Run any command or script in this terminal.
#. While the command is running, disconnect the terminal with ``<CTRL> + a`` and ``<CTRL> + d``.
#. Run ``screen -ls`` to list all virtual terminals.
#. Reconnect with ``screen -r``.

Common options::

* ``<CTRL> + a c`` Create a new window (with shell)
* ``<CTRL> + a “`` List all window
* ``<CTRL> + a 0`` Switch to window 0 (by number)
* ``<CTRL> + a A`` Rename the current window
* ``<CTRL> + a S`` Split current region horizontally into two regions
* ``<CTRL> + a |`` Split current region vertically into two regions
* ``<CTRL> + a tab`` Switch the input focus to the next region
* ``<CTRL> + a <CTRL> + a`` Toggle between the current and previous region
* ``<CTRL> + a Q``  Close all regions but the current one
* ``<CTRL> + a X`` Close the current region
	
	