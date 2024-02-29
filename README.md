<h1 align="center">openctp-ctpopt</h1>

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
:rocket:以 Python 的方式，简化对接 CTPAPI 股票期权的过程，节省精力，快速上手。

**openctp-ctpopt**是由[openctp](https://github.com/openctp)团队提供的官方ctpapi(c++)股票期权的python版本，
使用**swig**转换ctpapi(c++)生成。

---

* [支持版本](#支持版本)
* [使用方式](#使用方式)
    * [通过pip安装（推荐）](#通过pip安装)
    * [手动下载配置](#手动下载配置)
* [字符集问题](#字符集问题)
* [说明](#说明)

---

## 支持版本

| CTPAPI(C++) | openctp-ctp(python) | win x86            | win x64            | linux x64          |
|-------------|---------------------|--------------------|--------------------|--------------------|
| 3.7.0       | 3.7.0.*             | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| 3.6.7       | 3.6.7.*             | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| 3.6.3       | 3.6.3.*             | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| 3.5.8       | 3.5.8.*             | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |

## 使用方式

需要自行提前准备好 Python 环境

### 通过pip安装

选择一个版本安装，如 3.7.0

```shell
pip install openctp-ctpopt==3.7.0.* -i https://pypi.tuna.tsinghua.edu.cn/simple --trusted-host=pypi.tuna.tsinghua.edu.cn
```

引用方法:

```python 
from openctp_ctpopt import tdapi, mdapi
```

更多的接口使用方法参考 [代码示例](#代码示例)

### 手动下载配置

手动下载指定版本的动态库文件，并配置库路径。

- Windows

  因为 windows 下，不同的 python 版本编译的动态库之间不可共用，所以不同的 python 版本需要下载指定版本对应的动态库。

    - C++内置转码方式

      swig 转换时使用 C++ 内置方式进行 GBK 和 UTF8 的编码转换  
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
  选择一个ctpapi版本，如: 3.7.0
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

- 通过openctp-ctpopt库只能连接支持ctpapi(c++)股票期权**官方实现**的柜台
- openctp-ctpopt 只支持 ctpapi 生产版本，不支持评测版本
- 限于时间/精力有限，只是在模拟平台进行了简单的测试，若要通过 openctp-ctpopt
  使用CTPAPI所有的接口或用于生产环境，请自行进行充分测试
- 后续会完善更多的测试, 以及用于生产的验证

**使用 openctp-ctpopt 进行实盘交易的后果完全由使用者自己承担！！！**
