
##########
浏览器对象
##########

除了 HTML 文档之外, 浏览器对象也很重要.

window
======

window 对象代表当前浏览器窗口.

screen
======

screen 对象是设备的屏幕.

navigator
=========

navigator 对象存储了浏览器的设置.

location
========

location 表示当前标签的位置.

常用操作:

.. js:function:: location.assign(uri
.. code-block:: javascript

    /**
     * 让当前标签页跳转到新网址
     * 输入可以是相对路径
     */
    location.assign("https://github.com");

    // 刷新
    location.reload();
