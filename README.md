## The Environment Variable Mount Point Filesystem (EMPFS)
======

The Environment Variable Mount Point Filesystem (EMPFS) is a FUSE filesystem
that reads the specified environment variable from a process'
`proc/${PID}/environ` file.  The environment variable is set to a full
directory path; this path will then become the actual directory to which
requests to the mount point are redirected.

### Building
empfs requires the following packages to build on Ubuntu:
* libfuse-dev

### Usage
```
empfs -m mount-point -e VARIABLE
   
    mount-point  - The mount directory
    VARIABLE     - The environment variable pointing to the actual directory
```

### Example

Using bash as the shell:
<br>

```
$ cd /tmp
$ mkdir wendy; echo "wendy" > wendy/a.txt
$ mkdir peter; echo "peter" > peter/a.txt
$ mkdir floating
```

Confirm the mount point does not redirect:
```
$ ls /tmp/floating
$
```

Start the EMPFS dæmon, specifying the mountpoint and the environment variable:
```
$ /path/to/empfs -m /tmp/floating -e FLOAT
$
$ mount | grep empfs
empfs on /tmp/floating type fuse.empfs (rw,nosuid,nodev,user=xxx)
$ 
```

Set the environment variable to redirect to wendy:
```
$ export FLOAT=/tmp/wendy
$ cat /tmp/floating/a.txt
wendy
```

Change the environment variable to redirect to peter:
```
$ export FLOAT=/tmp/peter
$ cat /tmp/floating/a.txt
peter
```

To unmount the filesystem:
```
$ cd                           # anywhere outside /tmp/floating
$ fusermount -u /tmp/floating
```

Clean up after trying the example:
```
$ rm -r /tmp/wendy /tmp/peter /tmp/floating
$ unset FLOAT
```


### Thanks

I would like to thanks Paul Jolly and his [blog entry about trying to implement
variant symlinks using Go]
(http://blog.myitcv.org.uk/2014/03/18/using-process-namespaces-to-implement-variant-symlinks.html).
This gave us the idea of using a FUSE filesystem to solve our problems.

Secondly I would like to thank Joseph J. Pfeiffer for his ["Writing a FUSE
Filesystem: a tutorial"](http://www.cs.nmsu.edu/~pfeiffer/fuse-tutorial/).
Almost everything I now know about FUSE and implementing a FUSE filesystem came
from this tutorial.

