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

在 JavaScript 中, 你可以通过 HTML 元素的 标签名, id 属性, class 属性来查找 dom 节点.
之后, 可以使用一个变量来存储这个节点, 从而实现对 dom 节点的读取与修改.

除了以上三种方法之外, 还可以通过 CSS 选择器, XPath 来更精确地搜索一个节点.
在已经得到一个节点的情况下, 还可以查找到它的父节点和子节点.

现在, 随便打开一个喜欢的网页, 让我们跳转过去,
进入 Console, 在其中练习如何查找 HTML 文档中的元素.

一份 html 文档将会被解析成一个 document 对象, 你可以把它当作一个 ``<html> ... </html>`` 标签中的内容.
所有的 dom API 都集中在 document 对象的方法中. 对于查找元素这部分功能, 可以使用:

.. code-block:: javascript
    :caption: 查找 document 中的元素

    document.getElementById
    document.getElementsByClassName
    document.getElementsByName
    document.getElementsByTagName
    document.getElementsByTagNameNS

它们的作用就写在方法名上了, 不同多说了吧.

如果要使用 CSS 选择器来查找元素, 可以使用:

.. code-block:: javascript
    :caption: CSS Selector

    // 返回第一个匹配的元素
    document.querySelector
    // 返回所有匹配的元素组成的数组
    document.querySelectorAll

.. note:: 对于单个元素的查找, 可以不用手写选择器.

    在 F12 开发者工具 Elements 面板中,
    可以手动选中一个节点, 在右键菜单的 "Copy" 功能组中可以 "copy selector".
    你也可以直接使用 "Copy -> copy JS path" 直接复制成 ``document.querySelector`` 语句.

    XPath 同理.

如果使用 XPath 来查找元素:

.. code-block:: javascript
    :caption: XPath

    document.evaluate(/* XPath 表达式 */, /* 起始节点 */, /* 命名空间解析器 */, /* 返回类型 */, /* 返回值 */);

具体用法建议参考 https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Introduction_to_using_XPath_in_JavaScript.

在找到一个子节点后, 也可以继续使用 document 所支持的这些方法, 在子节点中继续寻找. 但不能使用这些方法:

.. code-block:: javascript

    .getElementById
    .getElementsByName

另外, 可以通过 ``.children`` 属性得到节点的所有子节点, ``.parentElement`` 来得到父节点.

修改 HTML 节点的属性与值
========================

每一个节点都可以使用 ``.getAttribute`` 和 ``.setAttribute`` 方法来读取或设置节点的属性.
或者也可以直接通过 ``.name`` 来访问属性名, 通过赋值号来设置属性的值:

例如, 要修改一个元素的 style 属性, 可以:

.. code-block:: javascript

    var _node = document.getElementsByTagName("h1");
    _node.setAttribute("style", "display: none;");
    // 另一种方法
    _node.style = "display: none;";

节点内部的值可以使用 ``_node.innerHTML`` 或 ``_node.innerText`` 访问, 前者会得到内部的 HTML 文本,
后者会得到节点内部去除了 HTML 标签的文本.

追加或删除节点
==============

可以使用 ``document.write`` 将当前文档的内容设置为输入值, 输入的参数为符合 HTML 语法的字符串.

如果要添加一个节点, 需要先创建它, 然后将它以现存的节点为锚, 添加到文档中.

1. 创建. 使用 ``document.create`` 方法创建一个元素. 输入的参数是该元素的标签名::

    var _node = document.create("p");
    _node;
    // <p></p>

2. 修改, 给元素添加属性和内部的值::

    _node.class = "test";
    _node.innerText = "Hello World";

3. 将元素插入到不同位置::

    // node 是一个已查找到的元素
    // parent 是 node 的父元素

    parent.appendChild(_node); // 添加到子元素末尾
    parent.insertBefore(_node, node); // 插入到 node 之前

4. 要删除元素, 就先查找到父节点, 然后调用 ``.removeChild`` 方法删除::

    parent.removeChild(node);
