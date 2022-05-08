---
title: re
tags: [python,re]
date: 2022-05-04 23:52:55
---

## 正则表达式

1. 替换 `re.sub`

* 全部替换
```
line = '1.特征 8'
big_tail = re.compile('\d+$') # 删除行尾的数字，例如页码
res = re.sub(big_tail, '', line, count=1) # 仅替换一次
print(res) # 
```
* 部分替换
```
big_11 = re.compile('([：；。:;]+)([a-zA-Z\d]+\))') # 替换 2) 
line = '包括：1)开门大吉；a)关门大吉'
print(re.search(big_11, line).group(0)) # 正则命中的所有 ：1)
print(re.search(big_11, line).group(1)) # 第一个括号  ：
print(re.search(big_11, line).group(2)) # 第二个括号 1)

res = re.sub(big_11, r'\1', line) # (替换掉后半段，但保留前半段)，用第一个括号的内容替换掉命中的所有
print(res) #  包括：开门大吉；关门大吉
```
* replace
有时候直接用replace
<!-- more -->
1. 匹配
* 仅找到一个结果
    
  * `re.match('', line)` # 从文本的起始位置开始匹配，否则None
  * `re.search()` # 可以扫描整个字符串并返回第一个成功的匹配

* 找到多个结果
  * `re.findall()` # 所有满足匹配条件的结果，并以列表的形式返回


参考：https://blog.csdn.net/weixin_39383896/article/details/99690266

1. 常见的正则
```
# 去掉行首的序号编号
big_8 = re.compile('[①②③④⑤⑥⑦⑧⑨⑩]')
# 行中
big_10 = re.compile('[•·～#［\uf0b7\uf0d8＝＊*]') # 替换特殊字符 上将
big_11 = re.compile('([：；。:;]+)([a-zA-Z\d]+\))') # 替换 2) 
big_12 = re.compile('\s*[（\(][a-zA-Z/\d\s]{2,}[）\)]\s*') # 删除如（RSI2 aaa）
big_url = re.compile('http[s]?\s*://(?:[a-zA-Z]|[0-9]|[\s$-_@.&+]|[!*\(\),]|(?:%[0-9a-fA-F][0-9a-fA-F]))+') # 删除网址
big_mail = re.compile('[a-zA-Z0-9\.\-]+@[\w\.]+\s*\w+') # 邮箱地址
big_tail = re.compile('\d+$') # 删除行尾的数字，例如页码
big_phone= re.compile('\d{3,}(-\d{3,})+') # 删除电话号码，可能会附带删除地址编号
```

### 参考资料
`re.sub()` 替换部分 https://blog.csdn.net/blmoistawinde/article/details/81839647

