# docker-python-multithreads

This dockerfile build to support module installation via pip and fork to run other .py.

## Build

1. Run the following command.
```bash
$ docker build -t local/python  .
```

## Run without Variable
In example, you can run start with simple docker by using this commond
```
$ docker run -d --name python local/python
```

## Output: log
This docker mount python-file in example to /usr/src/python by default.
The main.py try to install python module via pip and run p1.py, p2.py and scheduler.py
```
$ docker logs -f python
Installing PIP_MODULE:
You must give at least one requirement to install (see "pip help install")
PYTHON APP:True FILE:p1.py
parent: 1, child: 14

PYTHON APP:True FILE:p2.py
A new child: 14 run python file: /usr/src/python/p1.py
parent: 1, child: 15

PYTHON APP:True FILE:scheduler.py
A new child: 15 run python file: /usr/src/python/p2.py
parent: 1, child: 17

A new child: 17 run python file: /usr/src/python/scheduler.py
I'm p2.py
Traceback (most recent call last):
  File "/usr/src/python/scheduler.py", line 1, in <module>
    import schedule
ModuleNotFoundError: No module named 'schedule'
I'm p1.py
child pid: 14 is terminated
child pid: 15 is terminated
child pid: 17 is terminated
Exit main.py

```

## Run with Variable
You can specify python module with variable: PIP_MODULE. The PIP_MODULE is installed by command pip. 
Moreover, you can add sourcecode that you want to run by mapping /path/to/your/python to diractory:/usr/src/python
```bash
$ docker run -d --name python -e "PIP_MODULE=schedule" -v /path/to/your/python:/usr/src/python local/python
```

## Output: log
```bash
$ docker logs -f python
Installing PIP_MODULE: schedule
Collecting schedule
  Downloading schedule-0.4.2-py2.py3-none-any.whl
Installing collected packages: schedule
Successfully installed schedule-0.4.2
PYTHON APP:True FILE:p1.py
parent: 1, child: 14

PYTHON APP:True FILE:p2.py
A new child: 14 run python file: /usr/src/python/p1.py
parent: 1, child: 15

PYTHON APP:True FILE:scheduler.py
A new child: 15 run python file: /usr/src/python/p2.py
parent: 1, child: 17

A new child: 17 run python file: /usr/src/python/scheduler.py
I'm p2.py
start application
I'm p1.py
child pid: 14 is terminated
child pid: 15 is terminated
I'm working...
I'm working...
I'm working...
```
