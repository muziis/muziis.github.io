---
title: python_proxy
date: 2021-07-12 15:24:53
tags:
---

### Abuyun Proxy

```python
import base64


class BetterDict(dict):
    def __getattr__(self, item):
        return super().__getitem__(item)

    def __setattr__(self, key, value):
        super(BetterDict, self).__setitem__(key, value)


class Proxy(BetterDict):
    def __init__(self, username: str, password: str):
        super(Proxy, self).__init__()
        proxy = 'http://%(u)s:%(p)s@http-dyn.abuyun.com:9020' % {'u': username, 'p': password}
        self.http = proxy
        self.https = proxy


class AbuyunProxy(BetterDict):
    def __init__(self, username: str, password: str):
        super(AbuyunProxy, self).__init__()
        self.host = 'http://http-dyn.abuyun.com:9020'
        self.username = username
        self.password = password
        self.proxy: Proxy = Proxy(self.username, self.password)
        self.auth = base64.b64encode(('%(u)s:%(p)s' % {'u': self.username, 'p': self.password}).encode())
```

