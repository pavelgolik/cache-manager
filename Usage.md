The CacheManager client `cf` receives the requested file as command line argument. The path of the local copy is return on stdout.

# Retrieve Files from a File Server #
Simply replace the path to the used file with a call to `cf`
```
# instead of my-tool --input=/bigstorage/my/file.dat
my-tool --input=$(cf /bigstorage/my/file.dat)
```
In case the caching fails, `cf` will return the orginal path as fallback.

# Copy Files to a File Server #
```
output=/localstorage/my/generated/file.dat
cf -cp $output /bigstorage/my/generated/file.dat
```

# Caching Multiple Files #
Multiple files can be cached all at once by using "bundle" files.
Such a bundle file combines a set of files which are listed in a simple text file (one file including full path per line)
```
# list all used files
cat > files.bundle <<<EOF
/path/to/file/a
/path/to/file/b
/path/to/file/c
EOF

# copy all files in the bundle to the local hard disk
cachedBundle=$(cf files.bundle)

# $cachedBundle is a new bundle file on the local hard disk
# listing all cached files
```

Bundle files are expected to have a file name ending in `.bundle`.

# Configuration #
User-specific configuration file: `~/.cmclient`.

See also [Configuration](Configuration.md).

# Options #
  * get local copy: `cf [options] <filename>`
  * copy local file to file server: `cf [options] -cp [-n|--noregister] <source> <destination>`
> > `--noregister`: don't register local copy
  * retrieve locations of local copies: `cf [options] -l <filenames>`
  * options:
    * `--config <file>`: use alternative configuration
    * `--debug`: enable debug output
    * ` --bundle`: treat file as bundle
    * `--conjunct`: cache all files or none (for bundles)
    * `--nobundle`: ignore special meaning of .bundle files


# Python API #
The CacheManager offers its functionality also as a Python module
```

import cachemanager

...
# get path of the local copy
lf = cachemanager.cacheFile(fileServerPath)

# copy a local file to the file server
cachemanager.copyFile(lf, "/bigstorage/my/file")

# re-use connection to server for multiple files
config = cachemanager.client.ClientConfiguration()
config.loadDefault()
cachemanager.client.CmClient(config, False)

cachedFiles = []
for f in files:
    cachedFiles.append(client.fetch(f)
```



