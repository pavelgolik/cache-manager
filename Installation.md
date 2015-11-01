# Introduction #

CacheManager is written in Python. The server has to be installed on a
network reachable machine. It's recommended to use a dedicated machine and not to run it on one of your file servers. The client has to be installed on all cluster machines. The database of the server is kept in memory and gets stored frequently on disk.

The default configuration can be changed in the file
`settings.py`. Both server and clients have additional configuration
files. See also [Configuration](Configuration.md)

# Requirements #

  * Python 2.x (tested with Python 2.4 - 2.6)
  * SSH (if SSH connections are used for inter-node communication) with password-less login on all compute nodes

# Server #

Copy the files from the CacheManager package to the server machine,
e.g. in a directory `/opt/cache-manager`.

The server is started using `server.sh` which has to be modified to
define the installation directory.

You can use `sysvinit` to start the server at boot time. The script
assumes a system user `cmserver` and a group `cmserver` with write
permission for the [log file](LogFile.md) and the database file.

# Client #

The package has to be installed on all used compute nodes or
in a network reachable file system. The wrapper script `cf` can be
installed in a system directory like `/usr/local/bin`.

You can run the regression tests in `test.sh` to check your
installation. The header of `test.sh` has to be modified in order to
match your environemnt.