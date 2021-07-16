---
title: selenium_screenshot
date: 2021-07-16 11:07:29
tags:
---

```python
#!/usr/bin/python3
# -*- coding:utf-8 -*-
# @author : lee
# @project: Example
# @file   : ss.py
# @time   : 2021/6/28 13:15

import time
import os
from selenium import webdriver
from loguru import logger

EDGE = {
    "browserName": "MicrosoftEdge",
    "version": "",
    "platform": "WINDOWS",

    "ms:edgeOptions": {
        'extensions': [],
        'args': [
            '--headless',  # 无头
            '--disable-gpu',  # 禁用 gpu
            '--remote-debugging-port=9222',  # 远程端口
        ]
    }
}


class Scroll:
    def __init__(self, driver: webdriver.Edge):
        self.driver = driver

    @property
    def width(self):
        return self.driver.execute_script('return document.body.parentNode.scrollWidth')

    @property
    def height(self):
        return self.driver.execute_script('return document.body.parentNode.scrollHeight')


class ScreenShot:
    def __init__(self, driver: webdriver.Edge):
        self.driver = driver
        self.scroll = Scroll(driver)
        self.isBottom = False

    def get_html_height(self):
        return self.driver.execute_script('return document.body.clientHeight')

    def top_to_bottom(self):
        k = 1
        h = self.get_html_height()
        while True:
            if k * 500 < h:
                self.driver.execute_script('window.scrollTo(0,%s)' % (k * 500))
                time.sleep(0.2)
                h = self.get_html_height()
                k += 1
            else:
                break
        self.driver.set_window_size(self.scroll.width, self.scroll.height)
        self.isBottom = True

    def run(self, filename: str, suffix: str = 'png', overwrite: bool = False):
        filename = filename + '.' + suffix

        if not self.isBottom:
            logger.debug('size: %s' % self.driver.get_window_size())
            logger.debug('size correct, waiting ...')
            self.top_to_bottom()
            logger.debug('size: %s' % self.driver.get_window_size())

        if os.path.exists(filename):
            if not overwrite:
                raise FileExistsError(filename + ' is exists')
            else:
                logger.warning(filename + ' is exists, overwrite file')

        self.driver.get_screenshot_as_file(filename)
        

if __name__ == '__main__':
    driver_ = webdriver.Edge(capabilities=EDGE
    driver_.get('http://www.baidu.com')
    ScreenShot(driver_).run()
```
