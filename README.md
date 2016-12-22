uvbook
======

An Introduction to libuv

**以下为[imnemo](http://www.oncoding.me)关于如何学习uvbook的一些总结补充**  

---
## 学习环境
1. 电脑 `mac book pro`
1. 系统 `OS X EI Capitan` 版本 `10.11.6`

## 编译示例代码
### 编译libuv源码
在`code/Makefile`里`make all`，可以看到编译时命令是`gcc $(CFLAGS) -o $$dir main.c $(UV_LIB) $(LIBS);`
`$(CFLAGS)`变量的值为`CFLAGS=-g -Wall -I$(UV_PATH)/include`，会包含`UV_PATH即libuv`目录下的`include`，这个
在源码目录`libuv`下已经有了
`$(UV_LIB)`变量值为`UV_LIB=$(UV_PATH)/.libs/libuv.a`，这是静态库，需要编译下`libuv`源码才会生成。因此，我
们先来把`libuv`源码编译了。关于什么是静态库，可以参考[linux下的o、a、so、la、lo文件](./reference/linux下的o、a、so、la、lo文件.md)
#### 准备工作
[源码下README](./libuv/README.md#build-instructions)文件里，对编译过程和方式写的很详细
两种方式，一种是`autotool`，一种是`gyp`。`gyp`的方式，还可以生成mac下`xcode`工程相关文件。
先看`autotool`，使用brew安装就好：

```shell
brew install automake
brew install libtool
```

#### 编译
按照readme里介绍的执行就好了：

```
sh autogen.sh
./configure
make
make check
make install
```

### 编译示例代码
```shell
cd code
make all
```

## 编译生成文档
### 安装工具
文档是`.rst`文件，即`reStructuredText`，构建工具是`sphinx-build`
#### 安装sphinx-build
参考[官网](http://www.sphinx-doc.org/en/1.5.1/index.html)，当前版本是`1.5.1`，[install page](http://www.sphinx-doc.org/en/1.5.1/install.html)
可以通过`pip`来安装：

```shell
pip install -U Sphinx
```

也可以通过`MacPorts`安装：

```shell
#If you use Mac OS X MacPorts, use this command to install all necessary software.
sudo port install py27-sphinx

#To set up the executable paths, use the port select command:
$ sudo port select --set python python27
$ sudo port select --set sphinx py27-sphinx
```
安装`MacPorts`的话，参见[官网](https://www.macports.org/install.php)，直接下载对应系统的安装包

#### 安装LaTeX
导出PDF需要。[官网](https://www.latex-project.org/)介绍`LaTeX`是科学文档的事实标准。安装参见[install](https://www.latex-project.org/get/)
mac下，应该安装的是[MacTex](http://www.tug.org/mactex/index.html)。可以选择[最小化安装](http://www.tug.org/mactex/morepackages.html)

### 编译
`Makefile`里，可以看出支持生成多种格式的文档，常用的就是`html` `pdf` `epub`
运行`make ${format}`即可，`make help`可以查看更多命令。

### 编译问题
#### sphinx.ext.mathjax: other math package is already loaded
打开`source/conf.py`，编辑一行配置，其实是去掉这个扩展：

```shell
+extensions = ['sphinx.ext.todo', 'sphinx.ext.mathjax', 'sphinx.ext.ifconfig']
-extensions = ['sphinx.ext.todo', 'sphinx.ext.pngmath', 'sphinx.ext.mathjax', 'sphinx.ext.ifconfig']
```



