---
title: python_fastapi
date: 2021-07-12 15:28:26
tags:
---

### log.yaml

```yaml
version: 1
disable_existing_loggers: False
formatters:
  timestamped:
    format: '%(asctime)s - %(name)s - %(levelname)s - %(message)s'
handlers:
  console:
    class: logging.StreamHandler
    level: INFO
    formatter: timestamped
    stream: ext://sys.stdout
root:
  level: INFO
  handlers: [console]
```

```bash
$ pip install pyyaml
$ uvicorn main:app --log-config=log.yaml
```

### debugger

```python
if __name__ == '__main__': 
    from uvicorn.config import LOGGING_CONFIG

    LOGGING_CONFIG["formatters"]["access"]["fmt"] = '%(asctime)s | ' + LOGGING_CONFIG["formatters"]["access"]["fmt"]
    LOGGING_CONFIG["formatters"]["default"]["fmt"] = '%(asctime)s | ' + LOGGING_CONFIG["formatters"]["default"]["fmt"]
    uvicorn.run("main:app", use_colors=True)
```

