# pwsh设置[^pwsh]

## sudo权限

### 安装scoop[^scoop]

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
Invoke-Expression (New-Object System.Net.WebClient).DownloadString('https://get.scoop.sh')
```

### powershell7管理员模式--sudo安装

```powershell
scoop install sudo --global
```

**注意：scoop安装的包可能存在版本错误的问题，手动修改包的json以及它的MD5即可**

powershell md5sum

```powershell
Get-FileHash xxxxx -Algorithm MD5 | Format-list
```

### 效果

等效于run as Administrator



[^pwsh]: [PowerShell/PowerShell: PowerShell for every system! (github.com)](https://github.com/PowerShell/PowerShell)
[^scoop]: [ScoopInstaller/Scoop: A command-line installer for Windows. (github.com)](https://github.com/ScoopInstaller/Scoop)

