# Local Setup

## Setup

First, create the virtual environment.
Then install `mkdocs` and `mkdocs-material`.
Last, setup the **site** with `mkdocs`.

```console
xx@aio3477:~/projects/lab21%
xx@aio3477:~/projects/lab21% mkdir venv
xx@aio3477:~/projects/lab21% python3 -m venv venv/lab21
xx@aio3477:~/projects/lab21% source venv/lab21/bin/activate
(lab21) xx@aio3477:~/projects/lab21%
(lab21) xx@aio3477:~/projects/lab21% pip install mkdocs mkdocs-material
...
...
(lab21) xx@aio3477:~/projects/lab21% pip freeze > requirements.txt
(lab21) xx@aio3477:~/projects/lab21% mkdocs new .
INFO    -  Writing config file: ./mkdocs.yml
INFO    -  Writing initial docs: ./docs/index.md
```

## Run/Test

```console
xx@aio3477:~/projects/lab21%
xx@aio3477:~/projects/lab21% source venv/lab21/bin/activate
(lab21) xx@aio3477:~/projects/lab21% mkdocs serve -a 0.0.0.0:8088
INFO    -  Building documentation...
INFO    -  Cleaning site directory
INFO    -  Documentation built in 0.29 seconds
INFO    -  [11:19:03] Watching paths for changes: 'docs', 'mkdocs.yml'
INFO    -  [11:19:03] Serving on http://0.0.0.0:8088/lab21/
^CINFO    -  Shutting down...
(lab21) xx@aio3477:~/projects/lab21%
(lab21) xx@aio3477:~/projects/lab21% deactivate
```

