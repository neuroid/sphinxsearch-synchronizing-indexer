sindexer
========

This is a wrapper for Sphinx (http://sphinxsearch.com) indexer, which
simplifies synchronization of indices across replicas.


Usage
-----

	usage: sindexer [options] [index, [index...]]

	optional arguments:
	  -h, --help            show this help message and exit
	  --data-dir PATH       force remote data directory
	  -n, --dry-run         do nothing, just print the commands that would get
	                        executed
	  -r [USER@]HOST, --remote [USER@]HOST
	                        send indices to a remote host (can be given multiple
	                        times)
	  --remove-suffix SUFFIX
	                        when renaming index files to *.new.* remove suffix
	                        from names
	  --wait SECONDS        number of seconds to wait for local index rotation
	                        when running with --rotate (default: 5))
	  -s, --sync-only       sync indices and reload, do not rebuild or merge
	  --ssh-key PATH        explicitly set a ssh key to use

	All options apart from the ones defined above are passed to the indexer.

The following commands should be available in $PATH: 
* local host: indexer, kill, rsync, ssh
* remote hosts: kill, mv, rename


Examples
--------

Rebuild all local indices (live) and synchronize with two remote hosts:

	sindexer --all --rotate -r 10.0.0.2 -r 10.0.0.3

Synchronize previously built indices:

	sindexer --all -s -r 10.0.0.4

Merge two indices and synchronize the resulting one:

	sindexer --merge data data-new --rotate -r 10.0.0.2 -r 10.0.0.3 -r 10.0.0.4

It may be desired to build indices with a predefined suffix in file names and
strip the suffix during synchronization. This can be achieved as follows:

	sindexer --config /etc/sphinxsearch/sphinx-build.conf data-build --remove-suffix=-build -r 10.0.0.2
