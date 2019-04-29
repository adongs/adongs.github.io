---
layout: post
title: "vsocde配置java和golang"
date: 2019-04-17 14:58:15
image: 'https://adongs.github.io/assets/img/resources/vscode.jpeg'
description: vsocde配置java和golang
category: 'vscode'
tags:
- vscode
- vscode java
- vscode golang
introduction: vsocde配置java和golang
---

## 安装vscode

#### 1.下载vscode


[vscode下载地址](https://code.visualstudio.com/)

#### 2.修改vscode为中文

> 快捷键 【Command+Shift+P】
![placeholder](https://adongs.github.io/assets/img/blog/vscode/1.png "vscode")
> 这里选择其他语言,我这里安装zh-cn所以能看见
![placeholder](https://adongs.github.io/assets/img/blog/vscode/2.png "vscode")
> 选择中文(简体) 点击安装 (我这里安装过)
![placeholder](https://adongs.github.io/assets/img/blog/vscode/3.png "vscode")
> 重启vscode
![placeholder](https://adongs.github.io/assets/img/blog/vscode/4.png "vscode")

## 配置java环境

#### 1.搜索java插件,输入 java extension 

![placeholder](https://adongs.github.io/assets/img/blog/vscode/12.png "vscode")

#### 2.搜索spring boot 相关的插件,输入 spring boot

![placeholder](https://adongs.github.io/assets/img/blog/vscode/13.png "vscode")

#### 3.配置vocode
![placeholder](https://adongs.github.io/assets/img/blog/vscode/14.png "vscode")

```json
{
    "typescript.locale": "zh-CN",
    "files.autoSave":"onFocusChange",
    "editor.minimap.enabled": false,
    "window.zoomLevel": 0,
    "workbench.editor.enablePreview": false,
    "workbench.editor.enablePreviewFromQuickOpen": false,
    "editor.renderIndentGuides": false,
    "editor.highlightActiveIndentGuide": false,
    "java.home":"/Library/Java/JavaVirtualMachines/jdk1.8.0_202.jdk/Contents/Home",
    "maven.executable.path":"/usr/local/Cellar/maven/3.6.0/bin/mvn",
    "java.configuration.maven.userSettings":"/usr/local/Cellar/maven/3.6.0/libexec/conf/settings.xml",
    "maven.terminal.customEnv":[
        {
        "environmentVariable":"JAVA_HOME",
        "value":"/Library/Java/JavaVirtualMachines/jdk1.8.0_202.jdk/Contents/Home"
        }
    ],
    "explorer.confirmDelete": false,
    "editor.suggestSelection": "first",
    "vsintellicode.modify.editor.suggestSelection": "automaticallyOverrodeDefaultValue",
    "files.exclude": {
        "**/.classpath": true,
        "**/.project": true,
        "**/.settings": true,
        "**/.factorypath": true,
        "**/jmeter.log":true,
        "**/*.iml":true,
        "**/.idea":true
    },
    "java.jdt.ls.vmargs": "-noverify -Xmx1G -XX:+UseG1GC -XX:+UseStringDeduplication -javaagent:\"/Users/yudong/.vscode/extensions/gabrielbb.vscode-lombok-0.9.7/server/lombok.jar\" -Xbootclasspath/a:\"/Users/yudong/.vscode/extensions/gabrielbb.vscode-lombok-0.9.7/server/lombok.jar\"",
    "workbench.sideBar.location": "left",
    "breadcrumbs.enabled": false,
    "workbench.statusBar.visible": true,
    "terminal.integrated.cursorBlinking": true,
    "terminal.integrated.cursorStyle": "line",
    "terminal.integrated.scrollback": 1000000,
    "workbench.startupEditor": "newUntitledFile",
    "terminal.integrated.confirmOnExit": true,
    "terminal.integrated.copyOnSelection": true,
    "terminal.external.osxExec": "iTerm.app",
    "workbench.iconTheme": "vscode-icons",
    "xmlTools.xmlFormatterImplementation": "classic"
}
```



## 配置golang环境

#### 1.搜索插件go插件,并安装

![placeholder](https://adongs.github.io/assets/img/blog/vscode/5.png "vscode")

#### 2.安装go的环境插件(go插件会提示你安装,大多数都会安装失败)这里假设你已经安装好golang

> 按【Control+`】弹出终端
![placeholder](https://adongs.github.io/assets/img/blog/vscode/6.png "vscode")

> 输入 go env

![placeholder](https://adongs.github.io/assets/img/blog/vscode/7.png "vscode")

> 进入GOPATH 目录

![placeholder](https://adongs.github.io/assets/img/blog/vscode/8.png "vscode")

> 目录结构如下

![placeholder](https://adongs.github.io/assets/img/blog/vscode/9.png "vscode")

> 进入src(没有自己创建)执行 git clone https://github.com/golang/tools.git tools

![placeholder](https://adongs.github.io/assets/img/blog/vscode/10.png "vscode")

> 创建目录结构 src\golang.org\x\tools 把 tools里的全部文件拷贝进去 

![placeholder](https://adongs.github.io/assets/img/blog/vscode/11.png "vscode")

> 输入如下

```sehll
cd GOPATH
go get -u -v github.com/nsf/gocode
go get -u -v github.com/rogpeppe/godef
go get -u -v github.com/golang/lint/golint
go get -u -v github.com/lukehoban/go-find-references
go get -u -v github.com/lukehoban/go-outline
go get -u -v sourcegraph.com/sqs/goreturns
go get -u -v golang.org/x/tools/cmd/gorename
go get -u -v github.com/tpng/gopkgs
go get -u -v github.com/newhook/go-symbols

go install github.com/ramya-rao-a/go-outline
go install github.com/acroca/go-symbols 
go install golang.org/x/tools/cmd/guru 
go install golang.org/x/tools/cmd/gorename 
go install github.com/josharian/impl 
go install github.com/rogpeppe/godef 
go install github.com/sqs/goreturns 
go install github.com/golang/lint/golint 
go install github.com/cweill/gotests/gotests
//这一步不是必须的
mv GOPATH/bin/gocode  GOPATH/bin/gocode-gomod
//完成后就可以编写golang了
```
