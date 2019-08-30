Distributed Execution
=====================

[![GitHub License](http://img.shields.io/badge/license-Apache-blue.svg?style=flat)](https://www.apache.org/licenses/LICENSE-2.0)

A cross-platform reference implementation for processing commands over
a distributed, asynchronous queue. `dist_exe` provides a single producer
(server) many consumer (client) system.

This tool uses only standard library methods so there are no dependencies.
It has been tested on Linux, macOS, and Windows 10 in Python 3.7 environments.


Installation
------------

```
pip install git+https://github.com/glentner/dist_exe [--upgrade] [--user]
```


Usage
-----

The _server_ role reads from standard input and publishes to a queue. If you don't 
have something actively creating work a good approach is to create a _MANIFEST_ file
and use `tail -f` to wait for work to arrive.

```
user@host-01: ~ $ touch MANIFEST
user@host-01: ~ $ tail -f MANIFEST | dist_exe server -H 0.0.0.0 -p 5050
```

The _client_ role connects to the remote queue and pulls items off, executing them using the local shell.
You can specify a template for the command line invocation, otherwise the items are take as being the entire
invocation.

```
user@host-02: ~ $ dist_exe client -H host-01 -p 5050 -t 'echo {}'
```