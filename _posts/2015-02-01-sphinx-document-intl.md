---
layout: post
title: "sphinx文档国际化"
description: ""
date:   2015-02-01 15:11:02 +0800
categories: tech
---


#安装sphinx

	sudo pip install sphinx

没有pip 时，可以先安装pip

	easy_install pip


![image](http://sphinx-doc.org/latest/_images/translation.png)
#sphinx-intl 国际化处理工具


###1.安装 sphinx-intl
	
	pip install sphinx-intl 
	或
	easy_install sphinx-intl

###2. 添加配置到你的conf.py
 
	locale_dirs = [‘locale/‘]
	gettext_compact= False 

###3.提取文档翻译消息到pot 文件
	
	make gettext
输出结果，生成pot文件，保存_build/locale目录

###4. 安装/更新 locale_dir
	sphinx-intl update -p _build/locale -l zh_CN -l ja

###5. 翻译你的po文件。
	目录： .locale/<lang>/LC_MESSAGES/.

###6. 构建mo 文件 与翻译的文档 
	sphinx-into build
	make -e SPHINXOPTS=“-D language=‘zh_CN'” html

祝贺你! 你翻译的文档位于 _build/html 目录下.

#开始文档翻译

翻译位于 ./locale/zh_CN/LC_MESSAGES 目录下的po 文件 . 如  builders.po sphinx 文档 实例:
   
	# a5600c3d2e3d48fc8c261ea0284db79b
	#: ../../builders.rst:4
	msgid "Available builders"
	msgstr "<这里填写你的目标语言>"

另一种情况, msgid 是多行且包含 reStructuredText 语法:

	# 302558364e1d41c69b3277277e34b184
	#: ../../builders.rst:9
	msgid ""
	"These are the built-in Sphinx builders. More builders can be added by "
	":ref:`extensions <extensions>`."
	msgstr ""
	"FILL HERE BY TARGET LANGUAGE FILL HERE BY TARGET LANGUAGE FILL HERE "
	"BY TARGET LANGUAGE :ref:`EXTENSIONS <extensions>` FILL HERE."

Please be careful not to break reST notation. Most po-editors will help you with that.
