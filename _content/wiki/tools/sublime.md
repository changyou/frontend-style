# Sublime Text3

----------------

sublime text 前端必备开发利器

## 安装插件 ##


>直接安装：

* 下载安装包，解压到Packages目录 (菜单 -> preferences -> packages)

***

>Package Control组件安装：

* 安装Package Control

　　自动安装：
1. 通过 ctrl + ` 或者 View -> Show Console 菜单打开控制台
2. 粘贴对应版本的代码回车安装

> sublime text3
>>```
import  urllib.request,os;pf='Package Control.sublime-package';ipp=sublime.installed_packages_path();urllib.request.install_opener(urllib.request.build_opener(urllib.request.ProxyHandler()));open(os.path.join(ipp,pf),'wb').write(urllib.request.urlopen('http://sublime.wbond.net/'+pf.replace(' ','%20')).read())
```

> sublime text2
>>```
import  urllib2,os;pf='Package Control.sublime-package';ipp=sublime.installed_packages_path();os.makedirs(ipp)ifnotos.path.exists(ipp)elseNone;urllib2.install_opener(urllib2.build_opener(urllib2.ProxyHandler()));open(os.path.join(ipp,pf),'wb').write(urllib2.urlopen('http://sublime.wbond.net/'+pf.replace(' ','%20')).read());print('Please restart Sublime Text to finish installation')
```

　　手动安装：
1. Preferences -> Browse Packages
2. 进入Installed Packages/ 目录
3. 下载Package Control.sublime-package，并且复制文件到Installed Packages/ 目录
4. 重启Sublime Text

* 安装插件

1. 按下 Ctrl + Shift + P 调出命令面板
2. 输入 install 调出 Install Package 选项并回车，然后在列表中选中要安装的插件

## 常用插件 ##

- Package Manager

- LESS/CoffeeScript SyntaxHighlighter Less/Coffee 代码高亮

- Better Coffeescript

- Coffee Compile

- Emmet 提供一种简练的语法规则，立刻生成对应的HTML结构或CSS代码

- Trainling Spaces

- Sublime Linter 代码校验插件，支持多种语言

- Sublime Prefixr CSS3私有前缀自动补全插件

- Live Reload 即时刷新页面

- Code Formatter 代码格式化

- DocBlockr

- JSFormat JavaScript 代码格式化

- Git git相关插件

- Pretty JSON

- Angular

## 快捷键参考 ##

快捷键设置：
```
[
    { "keys": ["ctrl+d"], "command": "run_macro_file", "args": {"file": "res://Packages/Default/Delete Line.sublime-macro"} }
    ,
    { "keys": ["ctrl+shift+k"], "command": "find_under_expand" }
]
```

全局设置：
```
{
    "color_scheme": "Packages/Color Scheme - Default/Sunburst.tmTheme",
    "font_size": 11
}
```

## 实用技巧 ##

实用快捷键：
Alt + Shift + NUM (分屏)
