---
layout: "post"
title: "工标网爬虫"
date: "2018-03-25 00:05"
---

```python
# coding=utf-8
import requests
from openpyxl import Workbook, load_workbook
from lxml import etree

class csres:

    def __init__(self):
        self.temp_url = "http://www.csres.com/s.jsp?keyword={}"
        self.headers = {
            "User-Agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_0) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/61.0.3163.100 Safari/537.36"}
        self.wb = load_workbook("work.xlsx")
        self.num = 0


    def get_url_list(self):  # 获取url列表
        # Excel 提取code
        sheet = self.wb.get_sheet_by_name("工作表1")
        return [self.temp_url.format(i.value) for i in sheet['A']]

    def parse_url(self, url):  # 发送请求，获取html字符串
        r = requests.get(url, headers=self.headers)
        return r.content.decode('gbk')

    def get_content_list(self,html_str):
        html = etree.HTML(html_str)
        content = html.xpath("//thead[@class='th1']/tr[2]/td[5]/font/text()")[0]
        return str(content)

    def save_content_list(self,div_state, num):#save
        # 存入Excel
        sheet = self.wb.active
        sheet.title = "工作表1"
        sheet["B%d" % (num + 1)].value = div_state
        self.wb.save('work.xlsx')

    def run(self):  # 实现主要逻辑
        # 1.url_list
        url_list = self.get_url_list()
        # 2.遍历，发送请求，获取数据
        for url in url_list:
            html_str = self.parse_url(url)
            # 3.提取数据
            div_state = self.get_content_list(html_str)
            # 4.保 存
            self.save_content_list(div_state, self.num)
            self.num += 1


if __name__ == '__main__':
    csres = csres()
    csres.run()
```
