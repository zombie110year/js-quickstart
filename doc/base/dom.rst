############
文档对象模型
############

文档对象模型 ( Document Object Model )
是将 HTML 文档在 JavaScript 中解析为一个对象.

在 HTML 中引入 JavaScript
=========================

一份 HTML 文档, 主体是一个 html 元素,
在 html 下, 有 head 与 body 两个节点.

在 head 节点中, 常用于设置网页的各种元数据 (meta),
引入 (link) 样式表, 以及导入脚本文件 (script).

可以使用如下语句:

.. code-block:: html

    <script src="/_static/js/example.js async></script>
    <script src="https://cdn.js.org/a_script.js" async></script>

将指定路径下的 js 文件导入当前 html 文档.
js 文件中的所有符号都会被导入当前文档所处的作用域当中.
即便是从互联网上下载的 js 文件也是一样.

注意 ``async`` 属性. 定义 script 的 async 属性将会让浏览器异步加载该脚本,
在 head 中嵌入的 script 块建议使用该属性.
否则会在将脚本加载完成后再渲染文档,
在网络不通畅时导致长时间白屏.

在 body 中的 script 块由于特殊的加载机制可以不使用该属性.

查找 HTML 中的元素
==================

在这里, 有一份 `html 文档 </_static/dom/example.html>`_.
