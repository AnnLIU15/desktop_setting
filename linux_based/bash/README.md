# 对.bashrc的一些小修改

## debian系(Ubuntu)

效果图

<img src="https://github.com/AnnLIU15/script-setting/blob/master/linux_based/bash/img/README/bash_ubuntu.png" width="500">

```
if [ "$color_prompt" = yes ]; then
   PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]\t \d:\n \[\033[01;34m\]\w\[\033[00m\]\$ '
# PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '
else
   PS1='${debian_chroot:+($debian_chroot)}\u@\h \t \d :\n \w\$ '
# PS1='${debian_chroot:+($debian_chroot)}\u@\h:\w\$ '
fi
```

## centos(服务器没有sudo权力，下次再说)

