---
layout: post
title: 一种将eclipse的pydev环境配置成BLACK风格的方式
category: Hackers
---

## 分两步：
### 1.从eclipsemarket下载moonlight风格主题，安装并配置到eclipse中

+首先安装插件，可以直接将页面插件拖到eclipse中，会自动安装
+重启eclipse，菜单Window > Preferences > General > Appearance，选择MoonRise（standalone）
[参考](http://marketplace.eclipse.org/content/eclipse-moonrise-ui-theme)



### 2.导入pydev要使用的epf文件
文件可以本地创建，内容如下，假定命名为dark_theme.dpf:
打开菜单  File > Import > Preferences，导入dark_theme.dpf文件。
[参考](http://pydev.blogspot.com/2009/07/creating-dark-theme-and-exporting-and.html)

```
file_export_version=3.0

/instance/org.python.pydev/CODE_COLOR=180,180,180
/instance/org.python.pydev/DECORATOR_COLOR=127,159,191
/instance/org.python.pydev/NUMBER_COLOR=63,127,95
/instance/org.python.pydev/EDITOR_MATCHING_BRACKETS_COLOR=165,42,42
/instance/org.python.pydev/KEYWORD_COLOR=136,3,91
/instance/org.python.pydev/SELF_COLOR=10,10,10
/instance/org.python.pydev/STRING_COLOR=63,95,191
/instance/org.python.pydev/COMMENT_COLOR=154,154,184
/instance/org.python.pydev/BACKQUOTES_COLOR=165,42,42
/instance/org.python.pydev/PARENS_COLOR=127,0,85
/instance/org.python.pydev/OPERATORS_COLOR=47,47,47
/instance/org.eclipse.ui.editors/AbstractTextEditor.Color.SelectionForeground.SystemDefault=false
/instance/org.eclipse.ui.editors/AbstractTextEditor.Color.Background.SystemDefault=false
/instance/org.eclipse.ui.editors/AbstractTextEditor.Color.Background=0,0,0
/instance/org.eclipse.ui.editors/AbstractTextEditor.Color.Foreground.SystemDefault=false
/instance/org.eclipse.ui.editors/AbstractTextEditor.Color.Foreground=255,255,255
/instance/org.eclipse.ui.editors/AbstractTextEditor.Color.SelectionBackground.SystemDefault=false
/instance/org.eclipse.ui.editors/AbstractTextEditor.Color.SelectionBackground=0,0,136
/instance/org.eclipse.ui.editors/pydevOccurrenceIndicationColor=128,64,0
/instance/org.eclipse.ui.editors/lineNumberColor=255,255,255
/instance/org.eclipse.ui.editors/printMargin=true
/instance/org.eclipse.ui.editors/printMarginColor=255,0,0
/instance/org.eclipse.ui.editors/currentLineColor=70,70,70
/instance/org.eclipse.ui.editors/currentIPTextStyle=BOX
/instance/org.eclipse.ui.editors/currentIPIndication=true
/instance/org.eclipse.ui.editors/currentIPHighlight=false
/instance/org.eclipse.ui.editors/secondaryIPTextStyle=DASHED_BOX
/instance/org.eclipse.ui.editors/secondaryIPHighlight=false
/instance/org.eclipse.ui.editors/secondaryIPIndication=true

```
以上COLOR数据可以按你自己的要求修改，重启eclipse，打开py文件，可以看到生效的black风格的UI了。

