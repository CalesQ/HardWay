[详情](http://docscn.studygolang.com/doc/install#windows)
## 1. 卸载
先在程序中卸载掉。
要从你的系统中移除既有的Go安装，需删除 go 目录。 在 Linux、Mac OS X、和 FreeBSD 系统下通常为 /usr/local/go， 在 Windows 下则为 c:\Go。
你也应当从你的 PATH 环境变量中移除 Go 的 bin 目录。 在 Linux 和 FreeBSD 下你应当编辑 /etc/profile 或 $HOME/.profile。 若你是通过Mac OS X 包安装的 Go，那么你应当移除 /etc/paths.d/go 文件。 

## 2. 安装
对于Windows用户，Go项目提供两种安装选项（从源码安装除外）： zip压缩包需要你设置一些环境变量，而实验性MSI安装程序则会自动配置你的安装。

### MSI安装程序
打开此MSI文件 并跟随提示来安装Go工具。默认情况下，该安装程序会将Go发行版放到 c:\Go 中。

此安装程序应该会将 c:\Go\bin 目录放到你的 PATH 环境变量中。 要使此更改生效，你需要重启所有打开的命令行。

### Zip压缩包
下载此zip文件 并提取到你的自选目录（我们的建议是c:\Go）：

若你选择了 c:\Go 之外的目录，你必须为你所选的路径设置 GOROOT 环境变量。

将你的Go根目录中的 bin 子目录（例如 c:\Go\bin）添加到你的 PATH 环境变量中。

### 在Windows下设置环境变量
在Windows下，你可以通过在系统“控制面板”中，“高级”标签上的“环境变量”按钮来设置环境变量。 Windows的一些版本通过系统“控制面板”中的“高级系统设置”选项提供此控制板。

## 3. 配置
1. **环境变量**
在Windows系统上除了需要在Path变量里添加go路径，还需要再环境变量里添加**GOPATH**工作目录的路径，可以有多个目录，用“;”隔开