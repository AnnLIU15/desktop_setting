# WSL2-Ubuntu22-Python

在Windows上配置的虚拟环境bug多 (jupyter在某一台机子上老是配置不成功)，故打算用vscode romote+WSL的方法避免拆坑

## 0. 启用WSL

打开pwsh的管理员模式，输入下述指令

```
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux

dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart

dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart

wsl --set-default-version 2

```

查看可安装版本

```
wsl --list --online
```

下载最新版Ubuntu

```
Invoke-WebRequest -Uri https://aka.ms/wslubuntu2004 -OutFile Ubuntu.appx -UseBasicParsing

curl.exe -L -o ubuntu-2004.appx https://aka.ms/wslubuntu2004

```

安装驱动

```
https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi
```

## 0. 更改apt源

清华源

```
nano /etc/apt/sources.list

sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak 
sudo rm /etc/apt/sources.list
sudo nano /etc/apt/sources.list
```

```
# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-updates main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-backports main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-backports main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-security main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-security main restricted universe multiverse

# 预发布软件源，不建议启用
# deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-proposed main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-proposed main restricted universe multiverse
```

## 1. 下载git和anaconda

下载git

```
sudo apt install git -y
```

设置git

```
git config --global user.name "xxx"
git config --global user.email "xxx@xx.com"
```

下载anaconda

```
https://repo.anaconda.com/archive/Anaconda3-2022.05-Linux-x86_64.sh

ANACONDA_INSTALLER_SCRIPT=Anaconda3-2022.05-Linux-x86_64.sh
ANACONDA_PREFIX=/usr/local
wget https://repo.anaconda.com/archive/$ANACONDA_INSTALLER_SCRIPT
chmod +x $ANACONDA_INSTALLER_SCRIPT
./$ANACONDA_INSTALLER_SCRIPT -b -f -p $ANACONDA_PREFIX
```

开启conda默认

```
conda init
```

开启ssh服务

```
sudo ps -e | grep ssh
sudo /etc/init.d/ssh start
sudo apt-get install openssh-server
```

获取公钥

```
ssh-keygen -t rsa -C "xxx@xx.com"
cat id_rsa.pub
```

更改pip源

```
mkdir ~/.pip
sudo nano ~/.pip/pip.conf
# ctrl+shift+v下面的内容
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
[install]
trusted-host = https://pypi.tuna.tsinghua.edu.cn
```

conda换源

```text
sudo nano ~/.condarc

ssl_verify: true
show_channel_urls: true
default_channels:
  - http://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
  - http://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
  - http://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2
custom_channels:
  conda-forge: http://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  msys2: http://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  bioconda: http://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  menpo: http://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch: http://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  simpleitk: http://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
```

[从零开始ubuntu18深度学习配置-1 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/359512841)

conda&pip命令来安装reqs

```text
while read requirement; do conda install --yes $requirement || pip install $requirement; done < requirements.txt
```

## 2. 搭配VScode

下载Remote - WSL插件即可

## 3. ZSH

```
sudo apt install zsh
chsh -s /bin/zs    (后面重启terminal，点0)
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
cd ~/.oh-my-zsh/themes
cp /home/xxx/.oh-my-zsh/themes/ys.zsh-theme /home/xxx/.oh-my-zsh/themes/ys_t.zsh-theme
nano /home/xxx/.oh-my-zsh/themes/ys_t.zsh-theme
```

在最后加上(执行时间)

```
function preexec() {
  timer=${timer:-$SECONDS}
}

function precmd() {
  if [ $timer ]; then
    timer_show=$(($SECONDS - $timer))
    if [[ $timer_show -ge $min_show_time ]]; then
      RPROMPT='%{$fg_bold[red]%}(${timer_show}s)%f%{$fg_bold[white]%}[%*]%f %{$reset_color%}%'
    else
      RPROMPT='%{$fg_bold[white]%}[%*]%f'
    fi
    unset timer
  fi
}

autoload -Uz add-zsh-hook
add-zsh-hook preexec preexec
add-zsh-hook precmd precmd
```

更换ZSH主题

```
nano ~/.zshrc

ZSH_THEME="ys_t"
```

## 4. 安装jupyter

下载

```
conda install jupyter notebook -y
```

