## The Variant Symlink Filesystem
======

The variant symlink filesystem is a fuse filesystem that reads the specified environment variable from a processes `proc/${PID}/environ` file.
The contents of this environment variable should be a full path. This path will then become the root path upon which all requests to the fuse 
mount point will be appended.

### Usage
```
vsfs mount-point ENVIRONMENT_VARIABLE
   
    mount-point           - The directory to which the variant symlink will be applied.
    ENVIRONMENT_VARIABLE  - The environment variable from which the root path will be obtained.
```

### Example

Our examples will use bash as the shell.


Set the environment variable, and re-exec bash to force the /proc/${PID}/environ file to reload and contain the ROOT_PATH environment variable
```
$ export ROOT_PATH=/home/cole
$ exec bash
$
```

Show the current listing of /home/cole
```
$ ls
Desktop  Documents  Downloads  lxc-test  Music  Pictures  Public  research  Templates  Videos  work
$
```

Show the current listing of mountdir (the mountpoint we will use)
```
$ cd mountdir
$ ls
$
```

Start the Variant Symlink Filesystem and specify the mountpoint and the environment variable to read the path from
```
$ vsfs mountdir ROOT_PATH
$
```

Change to the mountpoint and show that it is now a variant symlink based on the ROOT_PATH environment variable
```
$ cd mountdir
$ ls
Desktop  Documents  Downloads  lxc-test  Music  Pictures  Public  research  Templates  Videos  work
$
```

