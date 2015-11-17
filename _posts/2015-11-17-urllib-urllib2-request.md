---
layout: post
title: urllib, urllib2, requests
cover: cover.jpg
category: python
tags: [python, crawler]
---

## python3 变化

在python3中，urllib和urllib2 module已经被重新分配到了新的modules里

- urllib.request
- urllib.error
- urllib.parse

可以用[2to3](https://docs.python.org/2/glossary.html#term-to3)tool来转换

- `urllib.request.urlopen` 等同于 `urllib2.urlopen()`
- `urllib.urlopen()` 已经被remove了

### requests package

python doc里推荐使用`requests`建立higher-level http client interface

例子

```python
>>> r = requests.get('https://api.github.com/user', auth=('user', 'pass'))
>>> r.status_code
200
>>> r.headers['content-type']
'application/json; charset=utf8'
>>> r.encoding
'utf-8'
>>> r.text
u'{"type":"User"...'
>>> r.json()
{u'private_gists': 419, u'total_private_repos': 77, ...}
```

## urllib和urllib2主要差别

- urllib2 可以接受一个request object来设定url request的header, 而urllib只能接受url
- urllib 提供urlencode方法来构建`GET` query strings, 这是为什么他们俩总是混用的原因之一


## Reference

- [difference between urllib and urllib2](http://www.hacksparrow.com/python-difference-between-urllib-and-urllib2.html)
- [requests](http://requests.readthedocs.org/en/latest/)
