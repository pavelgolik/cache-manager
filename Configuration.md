Default configuration values are defined in `settings.py`. The client
uses user-specific configuration files, expected in `~/.cmclient`. The
server configuration is given as command line parameter for
`cm-server.py`

Configuration values may contain Python expressions and the variables
$(HOST) and $(USER) referring to hostname and username of the client
respectively.

# Client #
  * `MASTER_HOST`: hostname or IP of machine running the CacheManager server
  * `MASTER_PORT`: TCP port number of the CacheManager server
  * `CACHE_DIR`:  directory on local hard disks used for caching
  * `MIN_FREE`: minimum free space on cache disk (bytes). If this value is exceeded, the client tries to delete old files.
  * `MAX_USAGE`: maximum disk space used for caches per user (percent of total space). If this value is exceeded, the client tries to delete old files.
  * `MIN_AGE`:  minimum age (in terms of atime) of a file to be deleted (seconds)
  * `SOCKET_TIMEOUT`: timeout for the network connection to the master (seconds)
  * `STAT_TIMEOUT`: time to wait for status operations on remote files (seconds)
  * `IGNORE_BUNDLE`: ignore special meaning of bundle archives (.bundle) (boolean)

A template can be found in `cmclient.config`.

# Server #
  * `PORT`: TCP port number used
  * `CONNECTION_QUEUE`: queue size of the server socket
  * `MAX_COPY_SERVER`: maximum number of parallel transfers from/to file servers
  * `MAX_COPY_NODE`: maximum number of parallel transfers from/to compute nodes
  * `DB_FILE`: database persistence file
  * `DB_SAVE_INTERVAL`: interval between database writes (seconds)
  * `STAT_INTERVAL`: interval between statistics writes (seconds)
  * `CLEANUP_INTERVAL`: interval between database cleanups (seconds)
  * `SOCKET_TIMEOUT`: timeout for client sockets (seconds)
  * `MAX_WAIT_COPY`: maximum time a client may spend copying (seconds). If a client does not answer within this interval, the connection is assumed dead.
  * `CLIENT_WAIT`: time a client has to wait if the `MAX_COPY_SERVER` is reached before next copy attempt (seconds)
  * `MAX_AGE`: time after an unused record in the database is deleted (seconds)

A template can be found in `cmserver.config`.

# Inter-Node Communication #

The file system access across compute nodes is performed by default
using SSH. If all compute nodes can access the local hard disks of
each other using a network file system (e.g. NFS), you can define
another `ClientEnvironment` class (see `environment.py`) and change
the return value of `clientEnvironment` in `settings.py`.

# Multi-Environment Setup #

If the same client configuration is used in different environments,
i.e. with different CacheManager servers, cache directories, or
inter-node communication methods, you can define these environment
specific values in a new `ClientEnvironment` class. See
`environment.py` and `settings.multienv.py` for an example.