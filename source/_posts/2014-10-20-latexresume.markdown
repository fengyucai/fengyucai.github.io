--- 
layout: post
title: "简单几步生成自己的LaTeX简历"
date: 2014-10-20 22:31
comments: true
categories: latex
---

本文仅适用于OS X操作系统下的LaTeX简历生成。

##安装MacTeX
首先戳[MacTex.pkg 下载地址](http://mirror.ctan.org/systems/mac/mactex/MacTeX.pkg)下载并安装LaTeX。
没有安装Xcode的同学还要下载Xcode并安装Command Line Tools。


##修改LaTeX简历模版
首先在桌面新建一个后缀名为tex的文本文件，输入简历模版的内容，并替换修改成自己需要的信息。
这里以我的简历模版(resume-zn.tex)为例，读者可以自己在网上搜索更多的模版。

{% codeblock lang:latex %}
\documentclass[11pt,a4paper]{moderncv}

% moderncv themes
%\moderncvtheme[blue]{casual}                 % optional argument are 'blue' (default), 'orange', 'red', 'green', 'grey' and 'roman' (for roman fonts, instead of sans serif fonts)
\moderncvtheme[blue]{classic}                % idem
\usepackage{xunicode, xltxtra}
\XeTeXlinebreaklocale "zh"
\widowpenalty=10000

%\setmainfont[Mapping=tex-text]{文泉驿正黑}

% character encoding
\usepackage[utf8]{inputenc}                   % replace by the encoding you are using
\usepackage{CJKutf8}

% adjust the page margins
\usepackage[scale=0.8]{geometry}
\recomputelengths                             % required when changes are made to page layout lengths
\setmainfont[Mapping=tex-text]{Hiragino Sans GB}
\setsansfont[Mapping=tex-text]{Hiragino Sans GB}
%\setsansfont[Scale=MatchLowercase,Mapping=tex-text]{Gill Sans} % Font for your name at the top

\CJKtilde

% personal data

%% start of file `template-zh.tex'.
%% Copyright 2006-2012 Xavier Danaux (xdanaux@gmail.com).
%
% This work may be distributed and/or modified under the
% conditions of the LaTeX Project Public License version 1.3c,
% available at http://www.latex-project.org/lppl/.

% 个人信息
\firstname{XXX}
\familyname{}
\title{个人简历}                      % 可选项、如不需要可删除本行
\mobile{+86~0123456789}                         % 可选项、如不需要可删除本行
%\phone{+2~(345)~678~901}                          % 可选项、如不需要可删除本行
%\fax{+3~(456)~789~012}                            % 可选项、如不需要可删除本行
\email{i@caifengyu.net}                    % 可选项、如不需要可删除本行
\homepage{fengyucai.github.io}                  % 可选项、如不需要可删除本行
%\extrainfo{附加信息 (可选项)}                  % 可选项、如不需要可删除本行
%\quote{引言(可选项)}                           % 可选项、如不需要可删除本行

% 显示索引号;仅用于在简历中使用了引言
%\makeatletter
%\renewcommand*{\bibliographyitemlabel}{\@biblabel{\arabic{enumiv}}}
%\makeatother

% 分类索引
%----------------------------------------------------------------------------------
%            内容
%----------------------------------------------------------------------------------
\begin{document}
\maketitle

\section{教育经历}
\cventry{2011 -- 2015}{本科} {广东外语外贸大学 英语语言文化学院 语言信息管理专业}{}{}{}  % 第3到第6编码可留白

%\section{毕业论文}
%\cvitem{题目}{\emph{题目}}
%\cvitem{导师}{导师}
%\cvitem{说明}{\small 论文简介}

\section{社区}
\cvline{Blog}{\url{http://fengyucai.github.io}}
\cvline{GitHub}{\url{http://github.com/fengyucai}}

\section{技术相关}
\cvline{算法与数据结构}{熟悉常用的数据结构，具备基本的算法分析和设计能力。}
\cvline{编程语言}{C/C++ > Objective-C > Python > Scheme > Perl}
\cvline{编译原理}{理解现代编译器的设计和构建，能够手写常见程序语言的tokenizer和parser。}
\cvline{操作系统}{了解OS的基本理论，熟悉Unix命令行操作及相关开发工具, Mac OS X长期用户。}
\cvline{数据库}{MySQL, SQL Server}
\cvline{开发框架}{Django, Cocoa/Cocoa Touch}
\cvline{版本控制}{Git}
\cvline{编辑器/IDE}{Vim, TextMate, Xcode, Visual Studio}
\cvline{文档排版}{\LaTeX}


\section{语言技能}
\cvline{英语}{\textbf{TEM-4}，长期阅读英文技术类书籍和文档，能够与外国开发者无障碍地交流。}


\section{项目经历}
\renewcommand{\baselinestretch}{1.2}

\cventry{2014至今}
{C-minus Compiler}
{Compiler}
{个人项目}{}
{一个简化版的Ｃ编译器，作为学习编译原理之后的实践项目。独立实现了编译器前端部分的scanner, parser和semantic analyzer, 然后利用LLVM完成了后端的代码生成和优化等阶段。}


\cventry{2014}
{Zion: 基于互联网的英语创意写作平台}
{Python Django MySQL}
{团队项目}{}
{Zion是一个在线英语写作网站，旨在通过增加学生之间的互动来提高英语写作水平。我作为主程制定了各项开发细节(包括编码规范，测试框架等)，并实现了文章发布，用户评论，互相关注，文本的语法分析等主要功能。}


\cventry{2013}
{LittleLisper}
{Interpreter}
{个人项目}{}
{一个用Scheme编写的Scheme解释器，实用性较差，纯属自娱自乐。}

\cventry{2013}
{EnglishWordFinder}
{C++}
{课程项目}{}
{一个英语文章分析工具。主要功能包括词频统计，词性归类，词典查询等，并用C++实现了对每个单词的stemming和lemmatization(类似PythonNLTK中的lemmatize函数)}

\cventry{2013}
{基于Cocos2d-x的手机游戏}
{C++}
{课程项目}{}
{基于跨平台的开源游戏引擎Cocos2d-x与C++实现的打飞机小游戏，在Windows下利用Visual Studio开发并移植到iOS, Android和Windows Phone三个平台。}

\cventry{2012}
{在线问卷调查系统}
{C\# SQL Server}
{课程项目}{}
{这是一个基于ASP.NET的在线问卷调查系统。我独立实现了后端自动生成问卷的功能。}



\section{校园经历} % (fold)
\cventry{2011--2012}
{广东外语外贸大学学生资讯中心记者}{}{}{}{参加社团期间负责过几场重要的人物专访，多篇文章在校园媒体网站“广外地带”发布。}

\closesection{}                   % needed to renewcommands
\renewcommand{\listitemsymbol}{-} % change the symbol for lists

% 来自BibTeX文件但不使用multibib包的出版物
%\renewcommand*{\bibliographyitemlabel}{\@biblabel{\arabic{enumiv}}}% BibTeX的数字标签
\nocite{*}
\bibliographystyle{plain}
\bibliography{publications}                    % 'publications' 是BibTeX文件的文件名

% 来自BibTeX文件并使用multibib包的出版物
%\section{出版物}
%\nocitebook{book1,book2}
%\bibliographystylebook{plain}
%\bibliographybook{publications}               % 'publications' 是BibTeX文件的文件名
%\nocitemisc{misc1,misc2,misc3}
%\bibliographystylemisc{plain}
%\bibliographymisc{publications}               % 'publications' 是BibTeX文件的文件名

\end{document}


%% 文件结尾 `template-zh.tex'.
 
{% endcodeblock %}


##编译生成pdf简历

打开终端，进入tex文件所在的目录, 然后执行xelatex命令进行编译。
{% codeblock %}
cd ~/Desktop
xelatex resume-zn.tex
{% endcodeblock %}
不出意外的话名为resume-zn.pdf的简历就成功生成了。Done！
读者可以重复以上步骤修改到满意为止。我的简历模版效果如图：
{% img /images/resume.png %} 

