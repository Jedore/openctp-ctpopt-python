<h1 align="center">openctp-ctpopt</h1>
<img src="openctp.jpg" align="right" height="140"/>

<div>
    <a href="#"><img src="https://flat.badgen.net/badge/os/windows-x86/cyan?icon=windows" /></a>
    <a href="#"><img src="https://flat.badgen.net/badge/os/windows-x86_64/cyan?icon=windows" /></a>
    <a href="#"><img src="https://img.shields.io/badge/os-linux_x86_64-white?style=flat-square&logo=linux&logoColor=white&color=rgb(35%2C189%2C204)" /></a>
</div>
<div>
    <a href="#"><img src="https://flat.badgen.net/badge/python/3.7|3.8|3.9|3.10|3.11|3.12/blue" /></a>
    <a href="https://pepy.tech/project/openctp-ctpopt" ><img src="https://static.pepy.tech/badge/openctp-ctpopt" /></a>
    <a href="#" ><img src="https://flat.badgen.net/badge/license/BSD-3/blue?" /></a>
    <a href="#" ><img src="https://flat.badgen.net/badge/CI/success/green?icon=github" /></a>
</div>
<br>

由 [**openctp**](https://github.com/openctp) 团队提供, 使用Swig制作的Python版股票期权CTPAPI。

简化对接股票期权CTPAPI的过程，节省精力，快速上手 :rocket:

---

* [支持版本](#支持版本)
* [快速使用](#快速使用)
    * [方式一 pip](#方式一-pip)
    * [方式二 手动配置](#方式二-手动配置)
* [代码示例](#代码示例)
* [编码增强](#编码增强)
* [字符集问题](#字符集问题)
* [说明](#说明)

---

## 支持版本

| CTPAPI(C++) | openctp-ctpopt(python) | win x86            | win x64            | linux x64          |
|-------------|------------------------|--------------------|--------------------|--------------------|
| 3.7.0       | 3.7.0.*                | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| 3.6.7       | 3.6.7.*                | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| 3.6.3       | 3.6.3.*                | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| 3.5.8       | 3.5.8.*                | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |

## 快速使用

提供了两种安装使用方式: pip安装、手动配置。

*需要自行提前准备好 Python 环境*

### 方式一 pip

选择一个版本，如 3.7.0

```shell
pip install openctp-ctpopt==3.7.0.* -i https://pypi.tuna.tsinghua.edu.cn/simple --trusted-host=pypi.tuna.tsinghua.edu.cn
```

引用方法:

```python 
from openctp_ctpopt import tdapi, mdapi
```

### 方式二 手动配置

手动下载指定版本的动态库文件，并配置库路径。

- Windows

  因为 windows 下，不同的 python 版本编译的动态库之间不可共用，所以不同的 python 版本需要下载指定版本对应的动态库。

  如: 3.7.0-x64, python 3.10

  从目录 `3.7.0/win64` 和 `3.7.0/win64/py310` 下载库文件

  将下载的文件放在本地同一个目录下
  ```PowerShell 
  # 下载文件
  _thosttraderapi.pyd
  _thostmduserapi.pyd
  thosttraderapi.py
  thostmduserapi.py
  soptthosttraderapi_se.dll
  soptthostmduserapi_se.dll 
  ```

- Linux  
  选择一个期权ctpapi版本，如: 3.7.0

  从目录`3.7.0/linux64`下载所有的文件

  将下载的文件放在同一目录下
  ```bash
  _thosttraderapi.so
  _thostmduserapi.so
  thosttraderapi.py
  thostmduserapi.py
  libsoptthosttraderapi_se.so
  libsoptthostmduserapi_se.so
  ```
  将文件所在路径配置库路径(specify_path填写自己的路径)
  ```bash
  export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:<specify_path>
  ```

## 代码示例

参考 tdapi.py / mdapi.py

*通过pip安装的可以直接使用代码示例；手动安装配置的，需要修改一下引入方式, 是`import thosttraderapi`
而不是`import openctp_ctpopt`*

## 编码增强

在高级编辑器或IDE中，可以方便的查看接口说明及各字段含义。如下(Pycharm)
<div align="center"><img width="850" alt="" align="center" src="https://github.com/openctp/openctp-ctp-python/assets/17944025/e59a12c1-e5b5-40dd-a3e2-1d274c63bd69" /></div>
.
<div align="center"><img width="850" alt="" align="center" src="https://github.com/openctp/openctp-ctp-python/assets/17944025/1b06e371-3974-4e39-8328-34c12a1b0ae0" /></div>
.
<div align="center"><img width="850" alt="" align="center" src="https://github.com/openctp/openctp-ctp-python/assets/17944025/84e9e87c-1a8b-42f7-82d7-7e20829d2520" /></div>

## 字符集问题

Linux下安装后，需要安装中文字符集，否则导入时报错：

```text
>>> import openctp_ctpopt
terminate called after throwing an instance of 'std::runtime_error'
what():  locale::facet::_S_create_c_locale name not valid
Aborted
```

需要安装 `GB18030` 字符集，这里提供 ubuntu/debian/centos 的方案：

```bash
# Ubuntu (20.04)
sudo apt-get install -y locales
sudo locale-gen zh_CN.GB18030

# Debian (11)
sudo apt install locales-all
sudo localedef -c -f GB18030 -i zh_CN zh_CN.GB18030

# CentOS (7)
sudo yum install -y kde-l10n-Chinese
sudo yum reinstall -y glibc-common
```

## 说明

- 通过openctp-ctpopt库只能连接支持CTPAPI股票期权**官方实现**的柜台
- openctp-ctpopt 只支持CTPAPI生产版本，不支持评测版本
- 由于许多用户只要使用Python版CTPAPI即可，并不需要自己进行繁琐的编译工作(适配各种编译环境)
  ，且大量跨平台夸版本的编译分发相对麻烦，仅提供了编译好的pip包和pyd/so动态库。需要自行编译的用户可参考 [swig转换CTPAPI为Python攻略](https://www.jedore.top/blog/post/ctpapi-swig-python/),
  也可参考[景色的Python-CTPAPI](https://github.com/nicai0609/Python-CTPAPI)
- 可参考 [openctp-ctp](https://github.com/openctp/openctp-ctp-python)

**使用 openctp-ctpopt 进行实盘交易的后果完全由使用者自己承担！！！**
