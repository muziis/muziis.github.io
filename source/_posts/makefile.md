---
title: makefile
date: 2021-07-12 15:32:24
tags:
---

### fastapi common service

```makefile
PYTHON = venv/bin/python
PIP = venv/bin/pip
UVICORN = venv/bin/uvicorn
PORT = $(strip $(shell cat settings.py|grep PORT|awk '{$$1=""; $$2=""; print $0}'))
PROCESS_NAME = $(strip $(shell cat settings.py|grep PROCESS_NAME|awk '{$$1=""; $$2=""; print $0}'))
PROCESS_ID = $(shell ps -ef|grep -v grep|grep $(PROCESS_NAME)|awk '{print $$2}')

help:
	@echo
	@echo "venv:"
	@echo "-   create venv"
	@echo "requirements:"
	@echo "-   install requirements"
	@echo "start:"
	@echo "-   start process"
	@echo "stop:"
	@echo "-   stop process"
	@echo "restart:"
	@echo "-   restart script"
	@echo "status:"
	@echo "-   show process state"

venv:
	python3 -m venv venv

requirements:
	$(PIP) install -r requirements.txt

start:
	nohup $(UVICORN) main:app --host 0.0.0.0 --port $(PORT) --log-config=log.yaml >>app.log 2>&1 &
	@echo "PROCESS NAME: $(PROCESS_NAME)"

stop:
	kill -9 $(PROCESS_ID)
	@echo "         killed pid : $(PROCESS_ID)"
	@echo "killed process name : $(PROCESS_NAME)"

restart: stop start
	@echo "Reload Success $(PROCESS_NAME)"

status:
	@echo "process name : $(PROCESS_NAME)"
	@echo "         pid : $(PROCESS_ID)"
ifneq ($(strip $(PROCESS_ID)),)
	@echo "      status : ● running"
else
	@echo "      status : ■ idle"
endif
```

