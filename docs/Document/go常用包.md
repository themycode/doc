---
title: go常用包
tags: go
notebook: wiki 
---


- [VS Code 开发环境所需支持模块，全部安装完成会生成 17 个可执行文件](#vs-code-开发环境所需支持模块全部安装完成会生成-17-个可执行文件)
- [go 代码注意事项](#go-代码注意事项)
  - [golang.org/x/](#golangorgx)
  - [go install](#go-install)
  - [其他包 (install || import)](#其他包-install--import)
  - [github.com/](#githubcom)
  - [非正常情况](#非正常情况)
  - [gocode （核心 - 语法相关）](#gocode-核心---语法相关)
  - [vscode 配置](#vscode-配置)
    - [- 最终情况](#--最终情况)
  - [补充信息](#补充信息)
  - [go mod & gopls](#go-mod--gopls)
  - [安装 gopls](#安装-gopls)
  - [配置 vscode gopls](#配置-vscode-gopls)
    - [最后](#最后)

## VS Code 开发环境所需支持模块，全部安装完成会生成 17 个可执行文件

## go 代码注意事项

0. channel 没有性能瓶颈,除非大规模亿万级dua
1. sync.pool 也经常会用到
2. <https://pkg.go.dev/golang.org/x/sync/errgroup#example-Group-JustErrors>;
 errgroup 多个业务处理场景数据处理,能按组进行管理,如果有报错会返回第一个报错
3. 学任何语言看官方文档,少走弯路不要看二手资料,
4. 平常基本用协程和channel,用轻量协程解决并发问题
5. goroutine 野生的都会停止,在工程里如果有worker可以用goroutine池,一般不需要做
6. goroutine用数组会有false sharing问题,需要在数组加 padding,如 [4433]padding
7. b站用的微服务 kratos

### golang.org/x/

需要放在GOPATH：/src/golang.org/x/目录下，分别将支持包clone下来

```git
git clone https://github.com/golang/tools.git （工具集以及包源代码）
git clone https://github.com/golang/lint.git （lint 大家都不陌生了）
git clone https://github.com/golang/mod.git （mod 模块支持）
git clone https://github.com/golang/xerrors.git （错误值转换包）

```

### go install

需要手动进行安装来获取支持模块。命令推荐在 %GOPATH% 下执行，不然可能会安装到其他路径。

```go
go install golang.org/x/lint/golint
go install golang.org/x/tools/cmd/guru
go install golang.org/x/tools/cmd/gorename
```

### 其他包 (install || import)

需要 install 或者 import 来进行使用，但更推荐使用 install 方式。安装方式同上。

```git
git clone https://github.com/golang/net.git （网络包）
git clone https://github.com/golang/sync.git （同步/原子包）
git clone https://github.com/golang/crypto.git （加密包）
git clone https://github.com/golang/debug.git （debug 工具包）
git clone https://github.com/golang/oauth2.git （oauth2 认证包）
git clone https://github.com/golang/protobuf.git （protobuf 协议包）
```

以上为额外的且常用的一些功能的源码扩展包，如有需要则可以考虑使用，当然也可以使用第三方实现的包。

### github.com/

需要放在 %GOPATH%/src/github.com/ 目录下。可直接通过 go get 方式安装

```go
go get -u -v github.com/uudashr/gopkgs/cmd/gopkgs
go get -u -v github.com/ramya-rao-a/go-outline
go get -u -v github.com/rogpeppe/godef
go get -u -v github.com/godoctor/godoctor
go get -u -v github.com/acroca/go-symbols
go get -u -v github.com/fatih/gomodifytags
go get -u -v github.com/haya14busa/goplay/cmd/goplay
go get -u -v github.com/davidrjenni/reftools/cmd/fillstruct
go get -u -v github.com/cweill/gotests/...（三个点不是错误，真的）
```

### 非正常情况

无法正常使用 go get 安装时，使用 clone 并通过 go install 方式安装，注意要在 %GOPATH% 下使用命令。

```go
go get -u -v github.com/sqs/goreturns （无法正常安装时使用以下命令）

git clone https://github.com/sqs/goreturns.git
go install github.com/sqs/goreturns.git

go get -u -v github.com/josharian/impl （无法正常安装时使用以下命令）

git clone https://github.com/josharian/impl.git 
go install github.com/josharian/impl
```

**dlv 比较重要，如果是 x64 系统的还是建议安装此模块，此模块可以通过开发工具或编辑器工具来直接 debug 调试 go 代码。**
（该支持模块只支持 x64 版本，千万注意系统版本和 Golang 版本）

```go
go get -u -v github.com/go-delve/delve/cmd/dlv （无法正常安装时使用以下命令）

git clone https://github.com/go-delve/delve.git 
（go-delve 目录需要手动创建，然后将 delve 放进去）
go install github.com/go-delve/delve/cmd/dlv
```

### gocode （核心 - 语法相关）

```go
现在用gopls代替其他插件在vscode中安装go插件就可以使用
**https://github.com/golang/tools/blob/master/gopls/README.md**
# go 语法与自动补全，gocode-gomod 需要使用 VS Code 提示安装完成。

https://github.com/stamblerre/gocode.git （支持模块）
go get -u -v github.com/stamblerre/gocode

https://github.com/mdempsky/gocode.git （Go > 1.8 不支持模块）
go get -u -v github.com/mdempsky/gocode

https://github.com/nsf/gocode.git （旧版本，且不再维护）
go get -u -v github.com/nsf/gocode

```

### vscode 配置

```json
{
    "go.buildOnSave": "workspace",
    "go.lintOnSave": "package",
    "go.vetOnSave": "workspace",
    "go.coverOnSave": false,
    "go.lintTool": "golint",
    "go.useCodeSnippetsOnFunctionSuggestWithoutType": true,
    "go.useCodeSnippetsOnFunctionSuggest": true,
    "go.autocompleteUnimportedPackages": true,
    "go.gocodePackageLookupMode": "go",
    "go.inferGopath": true,
    "go.docsTool": "gogetdoc",

    "go.formatTool": "goreturns",
}

```

以上是 我自己用得比较舒服的配置方案，当然各位小伙伴也可以修改为自己觉得舒服的配置，例如在配置中搜索 go.docsTool 就可以看到该配置的值有哪些，然后自己尝试即可。

#### - 最终情况

全部安装完成会在 %GOPATH%/bin 目录下，那么最终的 17 个模块为：

```sh
dlv.exe                              gomodifytags.exe
fillstruct.exe                       gopkgs.exe
go-outline.exe                       goplay.exe
go-symbols.exe                       gorename.exe
gocode-gomod.exe                     goreturns.exe
gocode.exe                           gotests.exe
godef.exe                            guru.exe
godoctor.exe                         impl.exe
golint.exe
```

### 补充信息

补充内容，也是近期写代码时候发现的问题

```md
guru 和 gogetdoc 都是代码描述提示，建议使用 gogetdoc
godoctor 不再更新可以不考虑安装使用，可替代是 golint
goimports 和 goreturns 是格式化和包引用管理，建议使用 goreturns
github.com/stamblerre/gocode 这个支持 gocode mod 的包有 Bug 牵引过多资源非常卡，占用大量 CPU 资源，不要安装使用！！！
```

如果是 go mod 模式开发推荐使用 gopls！！！，VS Code 插件发现 go 项目使用了 mod 模式后会提示你安装。

### go mod & gopls

如果你想使用 go mod，那么请先配置 GOPROXY  
国内镜像代理任选其一：

`七牛： GOPROXY=https://goproxy.cngoproxy.cn`
`阿里云：GOPROXY=https://mirrors.aliyun.com/goproxy`

配置 GOPROXY 配置方式 Windows

```go
# Windows 1
go env -w GO111MODULE=on
go env -w GOPROXY=https://goproxy.cn,direct

# Windows 2
# 1. 打开并编辑系统环境变量
# 2. 新增 GOPROXY 变量名
# 3. 填写变量值 https://goproxy.cn
# 4. 确定并重启控制台
```

配置方式 macOS & Linux

```sh
# macOS & Linux 1
$ export GO111MODULE=on
$ export GOPROXY=https://goproxy.cn

# macOS & Linux 2
$ echo "export GO111MODULE=on" >> ~/.profile
$ echo "export GOPROXY=https://goproxy.cn" >> ~/.profile
$ source ~/.profile
```

### 安装 gopls

配置完成 GOPROXY 后，才开始进行安装 gopls手动安装
`go get golang.org/x/tools/gopls@latest # 可获取最新版本的 gopls`

自动安装

打开 vscode
打开 vscode 控制台
输入 go mod init v2，使用 mod 的 v2 版本
等待 vscode 右下角提示有可以用的插件安装，注意识别仅为 gopls 的插件再按确定 install
等待命令自动执行完成

### 配置 vscode gopls

VS Code 需要修改一下配置 文件 -> 首选项 -> 设置 -> settings.json gopls 就可以正常工作了。

```Json
"go.useLanguageServer": true,
"[go]": {
    "editor.formatOnSave": true,
    "editor.codeActionsOnSave": {
        "source.organizeImports": true,
    },
    // Optional: Disable snippets, as they conflict with completion ranking.
    "editor.snippetSuggestions": "none",
},
"[go.mod]": {
    "editor.formatOnSave": true,
    "editor.codeActionsOnSave": {
        "source.organizeImports": true,
    },
},
"gopls": {
     // Add parameter placeholders when completing a function.
    "usePlaceholders": true,

    // If true, enable additional analyses with staticcheck.
    // Warning: This will significantly increase memory usage.
    "staticcheck": false,
},
```

#### 最后

由于 你懂的 原因，大部分插件是无法通过正常方式安装的，当然你是科学上网的 Geek 那么这些问题真的不用担心，也大概率不会碰到。
我也是在尝试的过程中碰到了很多问题，然后慢慢去了解、拦截数据、分析、问问老外，然后才把这些问题解决，最后能够通过正常方式让 VSCode 能够拥有完整的从支持插件来支持体验完整的 Golang 编写。
如果能够帮助你，我甚感欣慰，如果你在这个过程中还遇到其他问题，欢迎留言，并 请描述操作过程以及关键部分的截图 。尽可能的为你解答。

作者：林小帅
链接：<https://juejin.cn/post/6861581078721740807>
来源：掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
