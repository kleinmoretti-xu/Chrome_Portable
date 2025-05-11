[!CAUTION]
本仓库发布的内容仅用于学习和研究目的，不得将本仓库内容用于商业或者非法用途。否则，一切后果由您自行负责。
## 谷歌浏览器Google Chrome便携版制作
### 1.下载Chrome离线版本
> ##### 1.1.[GitHub 下载](https://github.com/Bush2021/chrome_installer)。<br/>
> ##### 1.2.官网下载
> > 1.2.1.访问[官网](https://www.google.com/chrome/)，并下拉至底部。<br/>
> > 1.2.2.点击支持下的[Chrome 帮助](https://support.google.com/chrome/?hl=zh-CN&rd=3#topic=7438008)。<br/>
> > ![1](/assets/1.png)
> > 1.2.3.在打开的页面搜索框输入**离线下载**并搜索。<br/>
> > 1.2.4.点击搜索结果的**下载和安装Google Chrome**下的**计算机**按钮。<br/>
> > ![2](/assets/2.png)
> > 1.2.5.选择对应的平台，以Windows为例，点击**Windows**展开项，找到**离线安装 Chrome**，点击**Chrome 安装程序**，在新打开的页面点击下载即可下载离线安装包。<br/>
> > ![3](/assets/3.png)
> > 直达[下载连接](https://www.google.com/intl/en/chrome/next-steps.html?standalone=1&statcb=1&installdataindex=empty&defaultbrowser=0)

### 2.下载[GoogleChromePortable](https://portableapps.com/apps/internet/google_chrome_portable)启动器
### 3.解压启动器和离线安装包
> 3.1.使用解压工具（7-zip）解压启动器，**非打开安装**。<br/>
> 3.2.提取**GoogleChromePortable.exe**、**help.html**、**Other\Source\GoogleChromePortable.ini**文件到安装目录。<br/>
> ![4](/assets/4.png)
> 3.3.在安装目录新建**App**文件夹。<br/>
> 3.4.使用解压工具打开并解压离线安装包，找到**Offline**文件夹下的**chrome_installer.exe**。<br/>
> ![5](/assets/5.png)
> 3.5.使用解压工具解压并打开**步骤4**的exe文件，提取**chrome.7z**文件。<br/>
> 3.6.解压**步骤5**提取的**chrome.7z**文件，把**Chrome-bin**文件夹解压至**步骤3**创建的**App**文件夹。<br/>
> ![6](/assets/6.png)

### 4.运行**GoogleChromePortable.exe**
### 5.设置为默认浏览器
> 方法来自作者@winhelponline的文章[**Register Google Chrome Portable with Default Apps or Default Programs**](https://www.winhelponline.com/blog/register-google-chrome-portable-with-default-apps-or-default-programs/)。 以下为运行脚本：
```vbscript
'Registers Google Chrome Portable with Default Programs or Default Apps in Windows
'chromeportable.vbs - created on May 20, 2019 by Ramesh Srinivasan, Winhelponline.com
'v1.1 13-June-2019 - Enclosed file name parameter in double-quotes.
'v1.2 10-Sept-2020 - Fixed ApplicationIcon path. And added other supported URL protocols.
'v1.3 23-July-2022 - Minor bug fixes.

Option Explicit
Dim sAction, sAppPath, sExecPath, objFile, oFSO, sbaseKey, sbaseKey2, ArrKeys, regkey
Dim WshShell : Set WshShell = CreateObject("WScript.Shell") 
Dim oFS0 : Set oFSO = CreateObject("Scripting.FileSystemObject")

Set objFile = oFSO.GetFile(WScript.ScriptFullName)
sAppPath = oFSO.GetParentFolderName(objFile)
sExecPath = sAppPath & "\GoogleChromePortable.exe"

'Quit if GoogleChromePortable.exe is missing in the current folder!
If Not oFSO.FileExists (sExecPath) Then
   MsgBox "Please run this script from Chrome Portable folder. The script will now quit.", _
   vbOKOnly + vbInformation, "Register Google Chrome Portable with Default Apps"
   WScript.Quit
End If

If InStr(sExecPath, " ") > 0 Then sExecPath = """" & sExecPath & """"
sbaseKey = "HKCU\Software\"

If WScript.Arguments.Count > 0 Then
   If UCase(Trim(WScript.Arguments(0))) = "-REG" Then Call RegisterChromePortable
   If UCase(Trim(WScript.Arguments(0))) = "-UNREG" Then Call UnregisterChromePortable
Else
   sAction = InputBox("Type REGISTER to add Chrome Portable to Default Apps." & _
   "Type UNREGISTER To remove.", "Chrome Portable Registration", "REGISTER")
   If UCase(Trim(sAction)) = "REGISTER" Then Call RegisterChromePortable
   If UCase(Trim(sAction)) = "UNREGISTER" Then Call UnregisterChromePortable
End If


Sub RegisterChromePortable
   sbaseKey2 = sbaseKey & "Clients\StartmenuInternet\Google Chrome Portable\"
   
   WshShell.RegWrite sbaseKey & "RegisteredApplications\Google Chrome Portable", _
   "Software\Clients\StartMenuInternet\Google Chrome Portable\Capabilities", "REG_SZ"
   WshShell.RegWrite sbaseKey & "Classes\ChromeHTML2\", "Chrome HTML Document", "REG_SZ"
   WshShell.RegWrite sbaseKey & "Classes\ChromeHTML2\AppUserModelId", "Chrome Portable", "REG_SZ"
   WshShell.RegWrite sbaseKey & "Classes\ChromeHTML2\Application\AppUserModelId", "Chrome Portable", "REG_SZ"
   WshShell.RegWrite sbaseKey & "Classes\ChromeHTML2\Application\ApplicationIcon", sExecPath & ",0", "REG_SZ"
   WshShell.RegWrite sbaseKey & "Classes\ChromeHTML2\Application\ApplicationName", "Google Chrome Portable", "REG_SZ"
   WshShell.RegWrite sbaseKey & "Classes\ChromeHTML2\Application\ApplicationDescription", "Access the internet", "REG_SZ"
   WshShell.RegWrite sbaseKey & "Classes\ChromeHTML2\Application\ApplicationCompany", "Google Inc.", "REG_SZ"
   WshShell.RegWrite sbaseKey & "Classes\ChromeHTML2\DefaultIcon\", sExecPath & ",0", "REG_SZ"
   WshShell.RegWrite sbaseKey & "Classes\ChromeHTML2\shell\open\command\", sExecPath & " -- " & """" & "%1" & """", "REG_SZ"
   
   WshShell.RegWrite sbaseKey2, "Google Chrome Portable Edition", "REG_SZ"
   WshShell.RegWrite sbaseKey2 & "Capabilities\ApplicationDescription", "Google Chrome Portable Edition", "REG_SZ"
   WshShell.RegWrite sbaseKey2 & "Capabilities\ApplicationIcon", sExecPath & ",0", "REG_SZ"
   WshShell.RegWrite sbaseKey2 & "Capabilities\ApplicationName", "Google Chrome Portable Edition", "REG_SZ"   
   
   
   ArrKeys = Array ("FileAssociations\.htm", _
   "FileAssociations\.html", _
   "FileAssociations\.shtml", _
   "FileAssociations\.xht", _
   "FileAssociations\.xhtml", _
   "FileAssociations\.webp", _
   "URLAssociations\ftp", _
   "URLAssociations\http", _
   "URLAssociations\https", _
   "URLAssociations\irc", _
   "URLAssociations\mailto", _
   "URLAssociations\mms", _
   "URLAssociations\news", _
   "URLAssociations\nntp", _
   "URLAssociations\sms", _
   "URLAssociations\smsto", _
   "URLAssociations\tel", _
   "URLAssociations\url", _
   "URLAssociations\webcal")
   
   For Each regkey In ArrKeys
      WshShell.RegWrite sbaseKey2 & "Capabilities\" & regkey, "ChromeHTML2", "REG_SZ"
   Next
   
   WshShell.RegWrite sbaseKey2 & "DefaultIcon\", sExecPath & ",0", "REG_SZ"
   WshShell.RegWrite sbaseKey2 & "shell\open\command\", sExecPath, "REG_SZ"
   
   'Launch Default Apps after registering Chrome Portable   
   WshShell.Run "control /name Microsoft.DefaultPrograms /page pageDefaultProgram"  
End Sub


Sub UnregisterChromePortable
   
   sbaseKey2 = "HKCU\Software\Clients\StartmenuInternet\Google Chrome Portable"
   
   On Error Resume Next
   WshShell.RegDelete sbaseKey & "RegisteredApplications\Google Chrome Portable"
   On Error GoTo 0
   
   WshShell.Run "reg.exe delete " & sbaseKey & "Classes\ChromeHTML2" & " /f", 0
   WshShell.Run "reg.exe delete " & chr(34) & sbaseKey2 & chr(34) & " /f", 0
   
   'Launch Default Apps after unregistering Chrome Portable   
   WshShell.Run "control /name Microsoft.DefaultPrograms /page pageDefaultProgram"   
End Sub
```
> 用法：
> 1.将上述 VBScript 代码复制到记事本，并将文件另存为 chromeportable.vbs。
> 2.将文件移动到GoogleChromePortable文件夹中，以便其正常工作。
> 3.双击chromeportable.vbs运行它。
> 4.键入**REGISTER**并单击OK将Chrome Portable添加到默认应用程序。
> 5.该脚本会自动启动Default Apps或Default Programs。从列表中选择Google Chrome Portable并将其设置为默认值。
> 6.要从默认应用程序中删除Google Chrome Portable，请重新运行脚本，键入**UNREGISTER**并单击确定。

[!TIP]
文件目录结构为：
> 你的安装目录 <br/>
> |—App <br/>
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|—Chrome-bin <br/>
> |—chromeportable.vbs <br/>
> |—GoogleChromePortable.exe <br/>
> |—GoogleChromePortable.ini <br/>
> |—help.html <br/>
