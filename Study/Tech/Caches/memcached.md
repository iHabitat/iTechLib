# Memcached

## Installing libevent
1. Download libevent from website: [libevent official site ][libevent] 
2. untar and `cd` into the directory.  

	``` shell
	$ ./configure --prefix=/usr/local/libevent && make
	$ sudo make install
	```
3. test if libevent installed:

	```
	$ ls -al /usr/local/ | grep libevent
	```

## Install Memcached

### Get Memcached
```
$ wget http://memcached.org/latest
$ tar -zxvf memcached-1.x.x.tar.gz
$ cd memcached-1.x.x
```

### Config options
```
$ ./configure --prefix=/usr/local/memcached --with-libevent=/usr/local/libevent
```

### Make and install
```
$ make && make test
$ sudo make install
```

### Test if memcached installed
```
$ ls -al /usr/bin/memcached
```

## Start/Stop memcached server

### Commandline
1. start
	```
	$ /usr/bin/memcached -d -m 128 -u root -p 11211 -c 256 -P /var/run/memcached1.pid
	```
to see whether started or not:
	```
	$ ps aux | grep memcached
	```
2. stop
	```
	$ /usr/bin/memcached -d -m 128 -p $1 -u root -vv >> /tmp/memcached.log 2>&1  
	```

## New configuring server
### Commandline arguments
When setting up memcached for the first time, you will pay attention to *-m*, *-d*, and *-v*.


[libevent]: http://libevent.org/

