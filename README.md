# Tnsping on pure python

- [Tnsping on pure python](#tnsping-on-pure-python)
  - [Usage](#usage)
  - [Examples](#examples)
  - [Tests](#tests)
    - [On Oracle Database](#on-oracle-database)
    - [On fake TNSPing responder](#on-fake-tnsping-responder)

Oracle TNSPING application, written on pure python

tnsping.py prints:

- stdout
  - time in seconds of ping
  - -1 if can't connect to database
- stderr
  - none
  - Exception class and exception caption if can't connect to database

## Usage

```text
usage: tnsping.py [-h] [--port [PORT]] host

positional arguments:
  host

options:
  -h, --help            show this help message and exit
  --port [PORT], -p [PORT]
```

## Examples

```bash
./tnsping.py localhost
0.001087312
```

```bash
./tnsping.py localhost -p 1522
<class 'ConnectionRefusedError'> [Errno 111] Connection refused
-1
```

```bash
./tnsping.py some_random_drop_host -p 1522
<class 'TimeoutError'> timed out
-1
```

## Tests

### On Oracle Database

Run Oracle Database XE (you may use `run_oracle_database.sh` script, but for script usage you need Oracle account with `container-registry.oracle.com` access)

After database come up run `pytest -v`

```bash
pytest -v
================================== test session starts ==================================
platform linux -- Python 3.10.5, pytest-7.1.2, pluggy-1.0.0 -- /usr/bin/python
cachedir: .pytest_cache
rootdir: /code/tnsping-py
plugins: betamax-0.8.1
collected 2 items                                                                       

test_tnsping.py::test_correct PASSED                                              [ 50%]
test_tnsping.py::test_cant_connect PASSED                                         [100%]

=================================== 2 passed in 0.01s ===================================
```

### On fake TNSPing responder

Run `fake_tnsping_responder.py`

After responder start run `pytest -v`

```bash
pytest -v
================================== test session starts ==================================
platform linux -- Python 3.10.5, pytest-7.1.2, pluggy-1.0.0 -- /usr/bin/python
cachedir: .pytest_cache
rootdir: /code/tnsping-py
plugins: betamax-0.8.1
collected 2 items                                                                       

test_tnsping.py::test_correct PASSED                                              [ 50%]
test_tnsping.py::test_cant_connect PASSED                                         [100%]

=================================== 2 passed in 0.01s ===================================
```

In fake_tnsping_responder logs you may see:

```text
Connection from 127.0.0.1, data: b'\x00W\x00\x00\x01\x00\x00\x00\x018\x01,\x00\x00\x08\x00\x7f\xff\x7f\x08\x00\x00\x01\x00\x00\x1d\x00:\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x190\x00\x00\x00\x8d\x00\x00\x00\x00\x00\x00\x00\x00(CONNECT_DATA=(COMMAND=ping))'
Connection from 127.0.0.1, data: b'\x00W\x00\x00\x01\x00\x00\x00\x018\x01,\x00\x00\x08\x00\x7f\xff\x7f\x08\x00\x00\x01\x00\x00\x1d\x00:\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x190\x00\x00\x00\x8d\x00\x00\x00\x00\x00\x00\x00\x00(CONNECT_DATA=(COMMAND=ping))'
```
