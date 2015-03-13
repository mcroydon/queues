This is a very simple API for interacting with lightweight queues.  Currently queues that speak the memcached protocol, Amazon's SQS service, and Beanstalkd are supported.  External backends can be easily defined and used as long as they conform to the BaseQueue API.

This library is designed to be configured either from a Django settings file or via environment variables.  Heavy lifting is done by one of the backend libraries:

| Backend | Library | License |
|:--------|:--------|:--------|
| sqs | [boto](http://code.google.com/p/boto/) | MIT |
| memcache`*` | [python-memcached](http://www.tummy.com/Community/software/python-memcached/) | PSF |
|  | [cmemcache](http://gijsbert.org/cmemcache/) | GPL |
| beanstalkd | [beanstalkc](http://github.com/earl/beanstalkc/tree/master) | APL-2.0 |
| redisd (as of [r12](https://code.google.com/p/queues/source/detail?r=12)/0.4) | [redis](http://code.google.com/p/redis/source/browse/#svn/trunk/client-libraries/python) | MIT |
| zenqueued (as of [r14](https://code.google.com/p/queues/source/detail?r=14)/0.4) | [zenqueue](http://github.com/disturbyte/zenqueue/tree/master) | MIT |
| dummy (as of [r16](https://code.google.com/p/queues/source/detail?r=16)/0.5) | [built-in](http://code.google.com/p/queues/source/browse/trunk/queues/backends/dummy.py) (useful for single-threaded testing) | MIT |

`*` Memcached is not supported for queueing. You must use a queue server that is compatible with the memcached protocol such as MemcacheQ etc.

Example usage:
```
$ export QUEUE_BACKEND=memcached
$ export QUEUE_MEMCACHE_CONNECTION=localhost:21122
$ python
>>> from queues import queues
>>> q = queues.Queue('myname')
>>> q.write('test')
True
>>> len(q)
1
>>> q.read()
test
>>> queues.get_list()
['myname']
```

Any comments or feedback would be appreciated.  Please note that the API may change in future versions based on feedback and use.