
---
title: Sublime Text3插件
date: 2016-08-19 12:49:00
tags: [Sublime Text3] 
---

# 插件安装

## *package control*

1.  安装Sublime Text3
2.  打开Sublime Text3，*Ctrl+~* 调出控制台，输入代码安装 *package control*
<!-- more -->
代码如下：
```python
    import urllib.request,os,hashlib; h = '2915d1851351e5ee549c20394736b442' + '8bc59f460fa1548d1514676163dafc88'; pf = 'Package Control.sublime-         package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)
```

3.  等待安装，关闭后重新打开，OK！

*****
![](http://images2015.cnblogs.com/blog/858833/201608/858833-20160819130547140-1068639085.png)


## 几个插件
安装步骤：
Preference-package control-install package-输入插件名称搜索

*   Markdown Editing 

*   Markdown Preview

    >  .md文件编写和在本地浏览器预览
    
    以上两个配置：
    1.  Preference-package setting-Markdown Priview-setting user
![](http://images2015.cnblogs.com/blog/858833/201608/858833-20160819130330031-914926432.png)

    2.  添加代码，用来本地浏览器预览
![](http://images2015.cnblogs.com/blog/858833/201608/858833-20160819130343718-290834882.png)

    ```
    "browser" : "D:/chrome/Google/Chrome/Application/chrome.exe"
    // "浏览markdown的浏览器的路径"
    ```
    3.  Preference -Key Bindings user，添加代码，快捷键f7预览
![](http://images2015.cnblogs.com/blog/858833/201608/858833-20160819130349796-1458967604.png)

```
    { "keys": ["f7"], "command": "markdown_preview", "args": {"target": "browser", "parser":"markdown"} },
```

*   BracketHighlighter

    >   成对匹配增强，并修改括号等的颜色


#### 参考资料
 * [官方指南](https://packagecontrol.io/installation#st3)