**参考网址：**

 https://docs.ohif.org/essentials/installation.html

**下载代码：**

`git clone https://github.com/StarwalkerZYX/OHIFViewers.git`

**安装Orthanc**

网站：https://www.orthanc-server.com/

下载地址： http://www.orthanc-server.com/static.php?page=resources

**安装Chocolatey**

https://chocolatey.org/install

`Set-ExecutionPolicy Bypass -Scope Process -Force; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))`

**安装Meteor**

Run this command using an Administrator command prompt:

`choco install meteor`

**Windows系统需要手工设置环境变量**

我的电脑-》系统属性-》高级-》环境变量，添加：

METEOR_PACKAGE_DIRS="../Packages"

参考：https://github.com/OHIF/Viewers/issues/181

**安装OHIFViewer**

打开Powershell终端

```powershell
cd d:\GitHub\OHIFViewer\OHIFViewer
meteor npm install
meteor --settings ../config/orthancDICOMWeb.json
```

升级依赖包（可选）

```powershell
meteor update
```





