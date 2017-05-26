# docker-python-multithreads

This dockerfile build to support module installation via pip and fork to run other .py.

## Build

1. Run the following command.
```bash
$ docker build -t  .
```

## Run

**Assumption**  
kibana running on host: kibana and listen port is 5601.
elasticsearch running on host: elasticsearch and listen port is 9200.
If you want to proxy from nginx to kibana, you run the following command.

```bash
$ docker run -d --name python -e "PIP_MODULE=schedule" -v /path/to/your/python:/usr/src/python lokios/python-multithreads
```

and then, you can access to `localhost:5602` or `localhost:9201` on browser.

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
