# cpptips README

这是一个c++编写辅助工具，包括代码补全、提示、代码跟踪、跳转等，可辅助开发者编写代码和查看代码。该项目中包含c++语法解释算法，但并不完美，有兴趣的朋友可获取并修改和调整，但我们非常希望能反过来完善该开源项目。
> 项目开源地址：https://github.com/widyhu2020/cpptips

 版权声明：
 > 该项目为开源项目，任何人均可以获取和修改（包括使用其中部分算法或者功能），但发布使用仅限于任何开源项目，不可用于具有商业性质的非开源项目。

## 功能介绍

目前主要功能包括：
- 1、不需要proto编译成.pb.h和.pb.cc文件就可以提示proto的定义
- 2、可以直接跳转到proto的定义，也可以调整函数变量等的定义
- 3、支持linux系统头文件提示和跳转
- 4、支持索引下所有命名空间、类、结构体、全局函数、枚举等前缀匹配
- 5、对象.或者->操作可以提示类的成员变量，最高可以提示5层继承关系

![avatar](https://s1.ax1x.com/2020/04/12/GLi2D0.gif)

# 快速开始
libs文件为编译生成文件夹，请勿修改，如果需要修改请修code-analyse下面的js文件，然后使用编译工具编程到libs目下，编译过程会将es6语法的js文件编译成es5语法，libs中js为最终执行的代码。

## 各平台使用说明

 所有平台，插件启动的时候会自动编译，但因cpu版本，操作系统差异太多，可能在有些操作系统中不兼容，导致插件不可用，可以安装下面方式进行编译配置。

### window
> 安装nodejs，安装python环境（2.7x版本）、需要安装Visual C++ build tools编译环境（安装vs2019，我们测试过程中发现vs2017，若你的环境为vs2017，请先升级到vs2019）
```shell
#安装编译环境，如果你安装有vs2019等ide，此步骤可跳过
npm install --global --production windows-build-tools
#进到工程根目录执行
npm install
#安装node原生模块编译工具
npm install node-gyp
#桌面系统vscode使用electron构造窗口程序，此时需要使用electron-rebuild来重新编译原生模块，因此需要安装electron-rebuid工具
npm install electron-rebuild
#编译better-sqlite3模块
.\node_modules\.bin\electron-rebuild.cmd -f -w better-sqlite3 -v electronversion
#electronversion为当前vscode使用的electron的版本号，获取方式见下tips
```
windows编译环境指引：https://github.com/nodejs/node-gyp#on-windows

### macOS
> 安装nodejs，如果没有python环境需要安装python（2.7x版本），需要xcode编译环境（不一定需要安装完整的xcode）
```shell
npm install node-gyp
#桌面系统vscode使用electron构造窗口程序，此时需要使用electron-rebuild来重新编译原生模块，因此需要安装electron-rebuid工具
npm install electron-rebuild
#编译better-sqlite3模块
.\node_modules\.bin\electron-rebuild -f -w better-sqlite3 -v electronversion
#electronversion为当前vscode使用的electron的版本号，获取方式见下tips
```

### 云开发机linux环境

>安装nodejs，一般带有python环境
```shell
#若使用的是linux桌面版本，使用与macOS一致
#若使用的远程开发，此时不是使用的electron版的nodejs，如需要重新编译远程机器能使用的版本，请参考如下步骤
#1.编译integer，在桌面系统中该模块会被自动编译，linux需要单独编译
cd node_modules/integer
vscode-node ../node-gyp/bin/node-gyp.js configure
vscode-node ../node-gyp/bin/node-gyp.js rebuild

#2.编译better-sqlite3
cd node_modules/better-sqlite3
vscode-node ../node-gyp/bin/node-gyp.js configure
vscode-node ../node-gyp/bin/node-gyp.js rebuild

#注意：
#vscode-node并非一个指令，是一个简写，真正执行的需要使用完整的指令，如下例如：
/home/widyhu/.vscode-server/bin/2aae1f26c72891c399f860409176fe435a154b13/node
#这里不能使用linux上本身安装的nodejs，vscod-server执行的时候使用的是自己的node版本，他并不会使用操作系统的nodejs版本
#怎么获取vscode运行环境的nodejs？见Tips
```
Tips：
- node版本，而非机器上安装的版本，具体版本信息插件启动的时候会打印。vscode启动的时候找到插件最开始的输出，找到execPath对应的值即为vscode运行的nodejs路径
- electronversion为编译vscode时候的版本号，启动插件时插件有打印相关信息

## 如何调试测试
工程中实现了4个调试launch，可在调试面板上手动切换调试对象
#### 启动客户端调试（不可调服务端）
> 启动客户端调试（不可调服务端）的情况下代码跟踪不会进入到服务器相关代码
#### 启动测试程序（code-analyse）
> 启动测试程序（code-analyse）目录下有个test.js，这个为调试测试入口，可以在这里写好测试入口，然后启动调试
#### 服务端并调试
>因服务端不能单独运行调试，这个项一般不用，留着的目的是给下面客户端+服务器一起调试使用
#### 客户端 + 服务器调试
> 客户端 + 服务器调试模式下，服务器和客户端的代码都可跟踪，但是进入的是编译完成之后的代码，如果想调试编译前的代码，启动测试程序（code-analyse）进行调试

### 此外，你还可以使用如下指令，来运行和测试任务进程
#### node code-analyse/src/worker/rebuildAllFileIndex.js
> 重新编译所见索引，用法见rebuildAllFileIndex.js文件中的主进程部分代码
#### node code-analyse/src/worker/unzipSystemIncludeWorker.js
> 解压和导入系统库头文件索引
##### node code-analyse/src/worker/makeOwnsMapByCpp.js
> 分析cpp文件
##### node code-analyse/src/worker/checkNeedUpdate.js 
> 检查更新

# 扩展配置
进入配置方式：
- 进入vscode插件管理，找到cpptips
- 鼠标邮件单击，选择Extensions Settings
- 可以针对User或者Workspace两个维度进行设置
- 如果需要修改，直接点金Edit in settings.json，默认会填入系统默认值，如果不需要可删除
> 配置文件解释
```json
{
	"cpptips.needLoadLinkDir": {
		"type": "array",
		"default": ["/comm", "/mmcomm", "/platform"],
		"description": "默认情况下软连接不计算索引，加入需要加载的白名单中！"
	},
	"cpptips.needLoadDir":{
		"type": "array",
		"default": [
			"/mmpay/",
			"/mmpaygateway/",
			"/mmtenpay/",
			"/comm/",
			"/mmcomm/",
			"/platform/",
			"/comm2/",
			"/appplatform/"
		],
		"description": "需要加载的目录，未配置进来将不会加载该目录，如果配置为空，则加载工程目录下全部目录！"
	},
	"cpptips.ignorDir":{
		"type": "array",
		"default": [
		],
		"description": "明确不创建索引的目录！needLoadDir配置需要加载，但是存在子目录不需要加载时，可以使用该配置项控制，非正则匹配"
	},
	"cpptips.ignoreFileAndDir": {
		"type": "array",
		"default": [
			"^[.~]{1,1}.{1,128}$",
			"^.*_tools_pb\\.(cpp|h)$",
			"^.*testimpl_pb\\.(cpp|h)$",
			"^.*\\.pb\\.(cc|h)$",
			"^(sk_|sm_)[a-z0-9_.]{1,128}$",
			"^mm3rd$",
			"^lib32$",
			"^lib64$",
			"^lib64_debug$",
			"^lib64_release$",
			"^lib32_debug$",
			"^lib32_release$",
			"^debug$",
			"^release$",
			"^win32$",
			"^bin$"
		],
		"description": "需要忽略的文件的名称或者正则表达式，该规则叠加在最后，符合该规则的文件或者文件夹都将不加载"
	},
	"cpptips.updateCheckIntervalTime" : {
		"type": "integer",
		"maximum": 2147483647,
		"default": 10000000,
		"description": "配置更新检查的间隔时间"
	},
	"cpptips.updateCheckUrl" : {
		"type": "string",
		"default": "http://cpptips.com:8888",
		"description": "配置更新检查时拉取的文件url地址"
	}
}
```

## Issues

https://github.com/widyhu2020/cpptips/issues
