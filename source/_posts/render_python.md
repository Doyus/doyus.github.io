---
title: 几种js渲染方式的demo
---

@[TOC](几种js渲染方式的demo)

# 使用selenium

```python
import selenium.webdriver
import time
import selenium.webdriver

class Spider(object):
    def __init__(self):
        self.spidername="xxx_com"
        self.chromedriver_path = Chrome_path.chrome_path()
        self.url = 'http://www.xxx.com'
        # self.chromedriver_path = r"D:\cz_project\singleSpider\tool\chromedriver_win32\chromedriver.exe"
        options = selenium.webdriver.ChromeOptions()
        # options.add_experimental_option("excludeSwitches", ["enable-automation"])
        # options.add_experimental_option('useAutomationExtension', False)
        options.add_argument('window-size=1200x2000')  # 指定浏览器分辨率
        options.add_argument('--disable-gpu')  # 谷歌文档提到需要加上这个属性来规避bug
        # options.add_argument('--hide-scrollbars')  # 隐藏滚动条, 应对一些特殊页面
        # options.add_argument('blink-settings=imagesEnabled=false')  # 不加载图片, 提升速度
        # options.add_argument('--headless')
        self.browser = selenium.webdriver.Chrome(options=options, executable_path=self.chromedriver_path)
        script = 'Object.defineProperty(navigator, "webdriver", {get:()=>false,});'
        self.browser.execute_script(script)
        
    def get_html(self, url):
        self.browser.set_window_size(1200, 1000)
        self.brower.get(url)
        time.sleep(0.8)
        detail_html = self.brower.page_source
        print(detail_html)

```

# 使用requests_html（基于pyppeteer）

```python
from requests_html import HTMLSession

class Spider():
    
    def __init__(self):
        self.session = HTMLSession()
        
    def get_html(self, url):
        obj = self.session.get(url)
        obj.encoding = 'utf-8'
        obj.html.render(sleep=0.1)
        print(obj.html.html)
```

# 使用splash

```python
import requests
class Spider()
    def __init__(self):
        self.script =  """
        splash:go(args.url)
        splash:wait(1)
        return splash:html()
        """
        
    def get_html(self):
        resp = requests.post('http://xxx.xxx.xxx.xxx:8050//run', json={
            'lua_source': self.script,
            'url': 'https://www.yuncaitong.cn/publish/demand.shtml'
        })
        png_data = resp.text
```

#  使用pyppeteer

```python
import asyncio
from pyppeteer import launch

class Spider():

    async def get_html(self, url):
        self.browser = await launch()
        page = await self.browser.newPage()
        await page.goto(url)
        content = await page.content()
        print(content)
        dimensions = await page.evaluate('''() => {
            return {
                width: document.documentElement.clientWidth,
                height: document.documentElement.clientHeight,
                deviceScaleFactor: window.devicePixelRatio,
            }
        }''')
        print(dimensions)
        await self.browser.close()


if __name__ == "__main__":
    spider = Spider()
    asyncio.get_event_loop().run_until_complete(spider.get_html("http://www.baidu.com"))
```

