# 基于[^Drake-vue]小修改的主题

因此README也是它的[本体](https://github.com/liangjingkanji/DrakeTyporaTheme/blob/master/README.md)的copy, 只在修改处增加

| 原作者                                                       | release version |
| ------------------------------------------------------------ | --------------- |
| [liangjingkanji (劉強東) (github.com)](https://github.com/liangjingkanji) | 2.7.2           |

##  Customize

### **设置字体**

如果需要自定义字体, 请修改CSS文件`第18行`字体名称列表替换为你想要的字体即可替换全部显示字体, 比如以下

```css
:root 
{
    --monospace: Hack, "Source Han Serif SC", "Fira Code", Menlo, "Ubuntu Mono", Consolas; /*code font*/
    --text-font: var(--monospace); /*default font*/
    --title-font: var(--monospace); /*title font*/
    \vdots
}
```

1. 字体可以同时设置多个, 如果第一个字体不支持中文, 则会采用第二个字体的中文

2. 如果第一个字体未安装则会使用后续字体



**字体大小**: <kbd>设置界面</kbd> -> <kbd>外观</kbd> -> <kbd>字体大小</kbd> -> <kbd>设置字体大小</kbd>

> 如果对配色不满意可以打开**`\*.css`**主题文件修改对应变量值, ==本主题可根据全局属性定制字体/配色==





| 推荐字体                          | 描述                      |
| ---------------------------------------------------------- | ---------------------------------------------- |
| [Hack: A typeface designed for source code (github.com)](https://github.com/source-foundry/Hack) | Hack(不习惯使用原作者推荐的JetBrains字体)      |
| [Fira Code](https://github.com/tonsky/FiraCode)       | Free monospaced font with programming ligature |
| [adobe-fonts/source-han-serif: Source Han Serif ](https://github.com/adobe-fonts/source-han-serif) | 思源宋体 |

## Install



- [x] 首先确定已安装[Typora](https://typora.io/)
- [x] 通过`设置 -> 外观 -> 打开主题文件夹`打开theme目录
- [x] 复制你想要的对应主题名称`*.css`后缀文件 到`theme`目录下然后重启, 选择菜单 -> 主题



## Preview

| 主题名称    | 预览图                            |
| ------------ | ------------------------------------------------------------ |
| drake-vue   | <img src="https://github.com/AnnLIU15/desktop_setting/blob/master/typora/img/README/thumbnail-vue.png" width="500"/> |

## License

```
Copyright (C) 2018 Drake, Inc.



Licensed under the Apache License, Version 2.0 (the "License");

you may not use this file except in compliance with the License.

You may obtain a copy of the License at



http://www.apache.org/licenses/LICENSE-2.0



Unless required by applicable law or agreed to in writing, software

distributed under the License is distributed on an "AS IS" BASIS,

WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.

See the License for the specific language governing permissions and

limitations under the License.
```

[^Drake-vue]:[liangjingkanji/DrakeTyporaTheme: Drake Light Dark Vue Ayu Material Black Purple Juejin Google style for typora-theme (github.com)](https://github.com/liangjingkanji/DrakeTyporaTheme#install)

