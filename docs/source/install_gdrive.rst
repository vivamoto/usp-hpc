Install Google Drive
====================

Since data in the server are deleted periodically, keeping an external backup makes the recover easy. Storing in Google Drive is handy since the USP account has unlimited storage. This section shows how to access Google Drive in lince with **gdrive**.

.. caution::
	Anyone with access to **gdrive** may access your files in Google Drive. If you have sensitive information in your USP account, create another Google account just to store HPC backups.
	
According to the developer, gdrive is no longer maintained although it's still possible to connect and sync folders with Google Drive.

Steps:

#. Create Google credentials
#. Download GDrive
#. Edit configuration
#. Code compilation
#. Access GDrive

First you need to create a credential in Google to give permission for an application access GDrive. When you do this, you'll receive an ID and a key. This step is done only once. Follow the instructions in this `site <https://github.com/mbrother2/backuptogoogle/wiki/Create-own-Google-credential-step-by-step>`_ .

Download GDrive
^^^^^^^^^^^^^^^

Run all Linux commands in the server. The GDrive code was written in ``go`` and may be downloaded from GitHub with this command::

	$ go get github.com/prasmussen/gdrive

Edit configuration
^^^^^^^^^^^^^^^^^^^^

The code will be downloaded to the folder ``~/go/src/github.com/prasmussen/gdrive``. Go to this folder, open the file ``handlers_drive.go`` with a text editor of your choice and update the following lines with the credentials you got from the previous step::

	const ClientId = "367116221053-7n0v**.apps.googleusercontent.com"
	const ClientSecret = "1qsNodXN*****jUjmvhoO"

Code compilation
^^^^^^^^^^^^^^^^

Now, compile the code in this folder with the following command. No message should appear after running it::

	$ go build


The ``gdrive`` file is created in this folder. Change permission to make it executable::

	$ chmod 755 gdrive

Access GDrive
^^^^^^^^^^^^^

Run the ``gdrive list`` command to start the authentication process::

	$ ./gdrive list
	Authentication needed
	Go to the following url in your browser:
	https://accounts.google.com/o/oauth2/auth?access_type=offl...
	Enter verification code: 
	
Copy and paste the link in your browser to allow access. After inserting the verification code, the root folder in your Google Drive is listed in terminal. Then, move the ``gdrive`` file to folder ``~/.local/bin``::

	$ mkdir ~/.local/bin
	$ mv ~/go/src/github.com/prasmussen/gdrive/gdrive ~/.local/bin

The complete list of options in ``gdrive`` is available at the developer's `website <https://github.com/prasmussen/gdrive>`_ .

Folder synchronization
^^^^^^^^^^^^^^^^^^^^^^

``gdrive`` creates a ``fileId`` for each file and folder stored in drive. Use this ``fileId`` to sync a folder. In this example, the  ``fileId`` is ``0B3X9GlR6EmbnOEd6cEh6bU9XZWM`` and the folder ``_release/bin`` is being synced.

Create directory on drive::

	$ gdrive mkdir drive-bin
	Directory 0B3X9GlR6EmbnOEd6cEh6bU9XZWM created

Sync to drive::

	$ gdrive sync upload _release/bin 0B3X9GlR6EmbnOEd6cEh6bU9XZWM
	Starting sync...
	Collecting local and remote file information...
	Found 32 local files and 0 remote files

	6 remote directories are missing
	[0001/0006] Creating directory drive-bin/bsd
	[0002/0006] Creating directory drive-bin/linux
	[0003/0006] Creating directory drive-bin/osx
	[0004/0006] Creating directory drive-bin/plan9
	[0005/0006] Creating directory drive-bin/solaris
	[0006/0006] Creating directory drive-bin/windows

	26 remote files are missing
	[0001/0026] Uploading bsd/gdrive-dragonfly-x64 -> drive-bin/bsd/gdrive-dragonfly-x64
	[0002/0026] Uploading bsd/gdrive-freebsd-386 -> drive-bin/bsd/gdrive-freebsd-386
	[0003/0026] Uploading bsd/gdrive-freebsd-arm -> drive-bin/bsd/gdrive-freebsd-arm
	[0004/0026] Uploading bsd/gdrive-freebsd-x64 -> drive-bin/bsd/gdrive-freebsd-x64
	[0005/0026] Uploading bsd/gdrive-netbsd-386 -> drive-bin/bsd/gdrive-netbsd-386
	[0006/0026] Uploading bsd/gdrive-netbsd-arm -> drive-bin/bsd/gdrive-netbsd-arm
	[0007/0026] Uploading bsd/gdrive-netbsd-x64 -> drive-bin/bsd/gdrive-netbsd-x64
	[0008/0026] Uploading bsd/gdrive-openbsd-386 -> drive-bin/bsd/gdrive-openbsd-386
	[0009/0026] Uploading bsd/gdrive-openbsd-arm -> drive-bin/bsd/gdrive-openbsd-arm
	[0010/0026] Uploading bsd/gdrive-openbsd-x64 -> drive-bin/bsd/gdrive-openbsd-x64
	[0011/0026] Uploading linux/gdrive-linux-386 -> drive-bin/linux/gdrive-linux-386
	[0012/0026] Uploading linux/gdrive-linux-arm -> drive-bin/linux/gdrive-linux-arm
	[0013/0026] Uploading linux/gdrive-linux-arm64 -> drive-bin/linux/gdrive-linux-arm64
	[0014/0026] Uploading linux/gdrive-linux-mips64 -> drive-bin/linux/gdrive-linux-mips64
	[0015/0026] Uploading linux/gdrive-linux-mips64le -> drive-bin/linux/gdrive-linux-mips64le
	[0016/0026] Uploading linux/gdrive-linux-ppc64 -> drive-bin/linux/gdrive-linux-ppc64
	[0017/0026] Uploading linux/gdrive-linux-ppc64le -> drive-bin/linux/gdrive-linux-ppc64le
	[0018/0026] Uploading linux/gdrive-linux-x64 -> drive-bin/linux/gdrive-linux-x64
	[0019/0026] Uploading osx/gdrive-osx-386 -> drive-bin/osx/gdrive-osx-386
	[0020/0026] Uploading osx/gdrive-osx-arm -> drive-bin/osx/gdrive-osx-arm
	[0021/0026] Uploading osx/gdrive-osx-x64 -> drive-bin/osx/gdrive-osx-x64
	[0022/0026] Uploading plan9/gdrive-plan9-386 -> drive-bin/plan9/gdrive-plan9-386
	[0023/0026] Uploading plan9/gdrive-plan9-x64 -> drive-bin/plan9/gdrive-plan9-x64
	[0024/0026] Uploading solaris/gdrive-solaris-x64 -> drive-bin/solaris/gdrive-solaris-x64
	[0025/0026] Uploading windows/gdrive-windows-386.exe -> drive-bin/windows/gdrive-windows-386.exe
	[0026/0026] Uploading windows/gdrive-windows-x64.exe -> drive-bin/windows/gdrive-windows-x64.exe
	Sync finished in 1m18.891946279s

Add new local file::

	$ echo "google drive binaries" > _release/bin/readme.txt

Sync again::

	$ gdrive sync upload _release/bin 0B3X9GlR6EmbnOEd6cEh6bU9XZWM
	Starting sync...
	Collecting local and remote file information...
	Found 33 local files and 32 remote files

	1 remote files are missing
	[0001/0001] Uploading readme.txt -> drive-bin/readme.txt
	Sync finished in 2.201339535s

Modify local file::

	$ echo "for all platforms" >> _release/bin/readme.txt

Sync again::

	$ gdrive sync upload _release/bin 0B3X9GlR6EmbnOEd6cEh6bU9XZWM
	Starting sync...
	Collecting local and remote file information...
	Found 33 local files and 33 remote files

	1 local files has changed
	[0001/0001] Updating readme.txt -> drive-bin/readme.txt
	Sync finished in 1.890244258s


List of options
^^^^^^^^^^^^^^^

Use the command ``gdrive help`` to list the available options::

	$ gdrive help
	gdrive usage:

	gdrive [global] list [options]                                 List files
	gdrive [global] download [options] <fileId>                    Download file or directory
	gdrive [global] download query [options] <query>               Download all files and directories matching query
	gdrive [global] upload [options] <path>                        Upload file or directory
	gdrive [global] upload - [options] <name>                      Upload file from stdin
	gdrive [global] update [options] <fileId> <path>               Update file, this creates a new revision of the file
	gdrive [global] info [options] <fileId>                        Show file info
	gdrive [global] mkdir [options] <name>                         Create directory
	gdrive [global] share [options] <fileId>                       Share file or directory
	gdrive [global] share list <fileId>                            List files permissions
	gdrive [global] share revoke <fileId> <permissionId>           Revoke permission
	gdrive [global] delete [options] <fileId>                      Delete file or directory
	gdrive [global] sync list [options]                            List all syncable directories on drive
	gdrive [global] sync content [options] <fileId>                List content of syncable directory
	gdrive [global] sync download [options] <fileId> <path>        Sync drive directory to local directory
	gdrive [global] sync upload [options] <path> <fileId>          Sync local directory to drive
	gdrive [global] changes [options]                              List file changes
	gdrive [global] revision list [options] <fileId>               List file revisions
	gdrive [global] revision download [options] <fileId> <revId>   Download revision
	gdrive [global] revision delete <fileId> <revId>               Delete file revision
	gdrive [global] import [options] <path>                        Upload and convert file to a google document, see 'about import' for available conversions
	gdrive [global] export [options] <fileId>                      Export a google document
	gdrive [global] about [options]                                Google drive metadata, quota usage
	gdrive [global] about import                                   Show supported import formats
	gdrive [global] about export                                   Show supported export formats
	gdrive version                                                 Print application version
	gdrive help                                                    Print help
	gdrive help <command>                                          Print command help
	gdrive help <command> <subcommand>                             Print subcommand help


Schedule daily backup
^^^^^^^^^^^^^^^^^^^^^

Schedule daily backup of your folder with ``crontab``. Use this template::

	$ crontab -e
	# Crontab - Crontab template to automate virtual world
	# Template from https://gist.github.com/bretonics/9a48a3b9ef32d93d15f45c3f007550b4
	# Andrés Bretón ~ http://andresbreton.com, dev@andresbreton.com
	#
	# ==============================================================================
	# .---------------- minute (0 - 59)
	# |  .------------- hour (0 - 23)
	# |  |  .---------- day of month (1 - 31)
	# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
	# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
	# |  |  |  |  |   + command
	# *  *  *  *  *  CMD
	# ==============================================================================
	#
	# Set Path
	PATH=/bin:/usr/bin:/usr/local/bin:/scratch/<USER_ID>
	#
	# Backup of work folder daily at 02:00 AM
	# (change NUSP, folder name and fileId)
	00 02 * * *   /scratch/<Seu NUSP>/.local/bin/gdrive sync upload _release/bin 0B3X9GlR6EmbnOEd6cEh6bU9XZWM   >   backup.log  2>&1

