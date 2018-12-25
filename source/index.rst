################################
JavaScript 快速上手
################################

.. important:: 本文源代码在 GitHub 开源

    项目地址: |project-address|

    欢迎 issue, fork, Pull Request.

JavaScript 是一门非常简单的语言, 要掌握它的基本用法甚至可以只花费一天的时间.

本文面对那些希望掌握一门能方便自己生活的工具, 或者希望能从 JavaScript 入门编程的人.

你可以从本文学到什么:

- JavaScript 基本语法 (三章)
- DOM 文档对象模型 (一章)
- 实践项目: (每实例一章)
    - 编写 UserScript 并用脚本管理器管理
    - 为浏览器编写插件: Chromium, Firefox
    - 为 VsCode 编写插件
    - 为静态网页添加效果
- 对 nodejs, typescript, vue.js, react, electron ... 产生大概的认识

好了, 让我们开始学习吧!

.. toctree::
    :maxdepth: 2
    :caption: 基础
    :numbered:

    grammar
    function
    object-oriented

.. toctree::
    :maxdepth: 1
    :caption: 实践
    :numbered:

    prac-*

.. toctree::
    :maxdepth: 1
    :caption: 扩展阅读
    :numbered:

    whatis-*

.. toctree::
    :maxdepth: 1
    :caption: 附录
    :numbered:

    ref-*

.. note:: 对贡献者

    本文采用 `Sphinx <https://sphinx-doc.org>`_ 作为生成工具, 使用 `reStructuredText <http://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html>`_ (以下简称 rst) 作为标记语言.

    希望参与该项目的人士, 可以先通过上面的链接学习一下, 也可以阅读我学习 rst 时的笔记 |note-for-rst|.

.. |project-address| replace:: https://github.com/zombie110year/js-quickstart

.. |note-for-rst| replace:: https://learn-rst.zombie110year.top
