# Situation #
The targeted cluster computer environment looks like this:

![http://www-i6.informatik.rwth-aachen.de/~rybach/cache-manager-architecture-small.png](http://www-i6.informatik.rwth-aachen.de/~rybach/cache-manager-architecture-small.png)

We have a couple of file servers with large storage backends and a
large number of compute nodes. All components are connect by (gigabit)
ethernet. The file server storage is available on all compute nodes
using a network file system like NFS. Each of the compute nodes has a
local (e.g. SATA attached) hard disk of customary size.

The file servers are considered reliable, whereas the storage on
compute nodes is expected to be unsafe.

# Problems #

  * Data-intense applications running in the cluster access the file server in massively parallel. The parallel I/O often exhausts the available bandwidth of the network and the storage connection.

  * High-performance storage solutions are expensive and incompatible with the need for large amounts of disk capacity under a limited budget.

  * Distributed file systems require modifications of the operating systems running on the compute nodes (e.g. file system drivers) or modifications of the applications running on the cluster, both modifications are  undesired.

  * The local hard disks of the compute nodes, which offer good performance (but relatively low capacity), remain unused.

# Solution #

  * Applications read and write data only from/to local hard disks. The data has to be transfered from and to the file server as part of the start-up and shutdown phase.

  * The access to the file server is managed by a central server instance - the CacheManager server.

  * Copy operations are performed by the CacheManager client, which communicates with the CacheManager server over the network.

  * Parallel access of the file server is limited to a certain number of connections at a time. If the maximum number of allowed connections is reached, client access is serialized.

  * Copies of files on local hard disks are registered with CacheManager server, such that requested file copies can be served  from other compute nodes if available, which reduces the load of the file server. Modifications are checked using file size and modification time.

  * Files on remote compute nodes can be accessed using NFS or SSH.

  * The CacheManager client can be used as a replacement for the  `cp` command in a scripting layer. For example:
```
   # retrieve /bigstorage/my/file.dat from file server and store it locally
   # generate output on local storage
   sort $(cf /bigstorage/my/file.dat) > /localstorage/${USER}/my/sorted.dat
   # copy (managed) output to file server
   cf -cp /localstorage/${USER}/my/sorted.dat /bigstorage/my/file.dat
```

  * Disk usage on the compute nodes is monitored and unused files are removed automatically.

  * Dedicated access control is not required by leveraging access control of the underlying file system.

# Table of contents #
  1. [Installation](Installation.md)
  1. [Configuration](Configuration.md)
  1. [Usage](Usage.md)
  1. Analyzing the [log file](LogFile.md)