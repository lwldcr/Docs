# Python QuickStart


## Why
Python has become one of the most popular programming languages since its birth especially after ML become more and more popular around these years.
Things happen with certain reasons.

* simple
* cross platform
* rapid developing
* glue
* easy deploying

## How
Refs

* [liaoxuefeng](http://www.liaoxuefeng.com/)
* [python doc](https://docs.python.org/2/)
* [python cookbook](http://baike.baidu.com/link?url=O0rlwd9D_JjgcbyOi6mEYMXXbLXVyOW77KjnQKoiwGSQlrHLwXTt3fEb_PYo6tdfXejcOvIOeJ30YSjAqjy7tVZ1klKd1B5VoaEMtE1rhsq)

## Hello World
As usual, we demostrate a "Hello, World" of python as your first python program:

```python2
#!/usr/bin/env python2
print "Hello, World!"
```

or Python3:

```python3
#!/usr/bin/env python3
print("Hello, World!")
```

## Python2 or Python3

Python 3 is not fully back-compatible, so python2 program may not run on python3. 

Fortunately, after several years of progressing Python 3 has become strong and powerful and many third party libs have moved to Python 3 from Python 2. 

I recommend new Pythoners choose Python 3 as your programing platform instead of Python 2.There are some significant improvements for Python 3 compared to Python 2, so experienced Python 2 programmers are reasonable to try Python 3.

## Language basics

* variables: no need to state

* dynamical type

	```
		a = 1
		print(type(a)) # int
		a = 1.0
		print(type(a)) # float
		a = "Hello"
		print(type(a)) # str
	```

* flow control: if elif else, while, for
* function: def funcname(arg1, arg2, *args, **kwargs)

	```
		def function_with_default_args(arg1, arg2, arg3=123):
			return arg1 + arg2 + arg3

		function_with_default_args(1, 3, 5) # arg3 will use the given arg: 5, so result is 9

		function_with_default_args(1, 3) # arg3 will use default: 123, result is 127
	```

* class: class MyClass(object):

	* `__init__(self, ...)` :  something like java's constructor, but is unique
	* old type class VS new type class
	* magic methods: `__str__`, `__len__`, `__enter__`, `__exit__`, etc
	* duck typing(generator, iterator, context, etc)
		
		> If it walks like a duck and quacks like a duck then it is a duck.

		```python2
			import mysql.connector

			class DBWrapper(object):
        		def __init__(self, conn_info):
            		self.conn = mysql.connector.connect(conn_info)	

				def __enter__(self):
        		    return self.conn

		        def __exit__(self, *args, **kwargs):
        		    self.conn.close()


			conn_info = {'host': 'xxx', 'port': 'xxx', 'username': 'xxx', 'passwd': 'xxx', 'db': 'xxx'}
  		  	with DBWrapper(conn_info) as db:
	        	db.execute("SELECT * from xxx")
			# you dont have to close connection any more, 
			#__exit__() will be called after executing and connection will be closed safely.
		```

* Pythonic:
	* generator: yield

		```python2
		def fibonacci(n):
			a, b = 1, 1
			while i < n:
				yield a
         		a, b = b, a + b

		fib = fibnacci(100)
        print(type(fib))
		for i in fib:
            print(i)
		```

		```
		<class 'generator'>
		1
		1
		2
		3
		5
		8
		13
		21
		34
		55	
		```

	* list comprehension
	
		```python2
		a = [1, 2, 3, 5, 6, 8, 9]
		b = [ x for x in a if a % 2 == 1] # filter odd numbers
		print b
		```
	
		```
		[1, 3, 5, 9]
		```

	* syntactic sugar: `with`, decorator, mixin

		```python2
			import time
			import functools

			def time_it(func):
				@functools.wraps(func)
				def wrapper(*args, **kwargs):
					start = time.time()
					ret = func(*args, **kwargs)
					end = time.time()
					print("executing function: {}, cost: {}".format(func.__name__, end - start))
                    return ret
				return wrapper

			@time_it
			def huge_number_cal():
				for i in range(1, 100000):
					num += i ** 5
		```

	* everything is an object: function, variable, class, object...

		```python2
			
			def adder(x=5):
				def add(y):
					return x + y
				return add

			f = adder(5)
			print(type(f)) 
			print(f(5))
		```

		output:

		```
			function
			10
		```
	* functional programming: filter, map, reduce
		
		```python2
		a = [1, 2, 3, 4, 5, 6]
		b = filter(lambda x: x % 2 == 1, a)
		c = [x for x in b]
		print(c)
		```
		output:

		```
		[1, 3, 5]
		```

## Traps and Pitfalls
* Performance related
	* threading

		> multi threading run within just one CPU Core, caused by GIL(Global Interpretor Lock)

	* multiprocessing
		> Use multi Cores, but processes' creating&management cost much
    
	* coroutine
		> userspace, lightweight threading,
		
		> has it's own stack, registers context
		
		> no System call, rapid switch between coroutines

		> gevent
	
* Confusing encoding/decoding
	
	```
	Traceback (most recent call last):
 	 File "makedb.py", line 33, in 
    	main()
  	File "makedb.py", line 30, in main
    	fp.write(row[1])
	UnicodeEncodeError: 'ascii' codec can't encode characters in position 0-78: ordinal not in range(128)
	```
	This is something may kill a new Pythoner with CJK language environment.

	Fortunately, python 3 has a very inspiring feature which makes all text-like variables with Unicode type.
	
	A simple but smart way to handle encoding problems is when store text into files, encode them into UTF-8 codec; when loading text into variables, decode them into Unicode.


## What we use Python do
* Webpage crawlers
	* scrapy
	* build crawler with httplib, urllib, etc

* Data processing
	* numpy
	* pandas
	* xlrd/xlwt
	* csv
	* etc

* DB
	* ORM: sqlalchemy
	* low-level:
		* mysql-connector
		* redis/mongodb/sqlite

* Web
	* Flask
	* Django

* Machine Learning
	* scikit-learn
	* gensim // Natural Linguistic Programming
	* jieba
	* nltk
	* Deep learning: tensor flow

* Message Queue
	* kafka/rabbitmq/nsq

* System admin
	* sys/os
	* exec

* Low level supporting
	* ctypes
	* C/C++ 

* Deploying
	* apache/nginx
	* uWSGI/gunicorn
	* supervisor

## Dev Tools
* vim
	* autocomplete
	* syntax checking
	* PEP 8
* pyflakes
* ipython
* ipython notebook 
* PyCharm by JetBrains
* Sublime Text

## Accessory
```python2
>>> import this
The Zen of Python, by Tim Peters

Beautiful is better than ugly.
Explicit is better than implicit.
Simple is better than complex.
Complex is better than complicated.
Flat is better than nested.
Sparse is better than dense.
Readability counts.
Special cases aren't special enough to break the rules.
Although practicality beats purity.
Errors should never pass silently.
Unless explicitly silenced.
In the face of ambiguity, refuse the temptation to guess.
There should be one-- and preferably only one --obvious way to do it.
Although that way may not be obvious at first unless you're Dutch.
Now is better than never.
Although never is often better than *right* now.
If the implementation is hard to explain, it's a bad idea.
If the implementation is easy to explain, it may be a good idea.
Namespaces are one honking great idea -- let's do more of those!
```
