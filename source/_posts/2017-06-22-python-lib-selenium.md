title: python-lib~selenium
date: 2017-06-22 09:55:03
category:
tags:
description:
---
[Toc]


### Driver安装

[Driver link](https://selenium-python.readthedocs.io/installation.html#drivers)

### 一份官方示例代码

``` python
from selenium import webdriver
from selenium.webdriver.common.keys import Keys

driver = webdriver.Firefox()
# driver = webdriver.Chrome()
driver.get("http://www.python.org")
assert "Python" in driver.title
elem = driver.find_element_by_name("q")
elem.clear()
elem.send_keys("pycon")
elem.send_keys(Keys.RETURN)
assert "No results found." not in driver.page_source
driver.close()
```

* [官方解释，就是牛](https://selenium-python.readthedocs.io/getting-started.html#)

### Locating Elements

[官方文档，就是牛](https://selenium-python.readthedocs.io/locating-elements.html#locating-elements)

There are various strategies to locate elements in a page. You can use the most appropriate one for your case. Selenium provides the following methods to locate elements in a page:

* find_element_by_id
* find_element_by_name
* find_element_by_xpath
* find_element_by_link_text
* find_element_by_partial_link_text
* find_element_by_tag_name
* find_element_by_class_name
* find_element_by_css_selector

To find multiple elements (these methods will return a list):

* find_elements_by_name
* find_elements_by_xpath
* find_elements_by_link_text
* find_elements_by_partial_link_text
* find_elements_by_tag_name
* find_elements_by_class_name
* find_elements_by_css_selector

### selenium.webdriver.common.keys

pass
