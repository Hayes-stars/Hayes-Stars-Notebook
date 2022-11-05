# 编程入门篇

## 体会编程是什么？

- [《与孩子一起学编程》](https://book.douban.com/subject/5338024/)，这本书以Python语言教你如何写程序，是一本老少皆宜的编程书。其中会教你编一些小游戏，还会和你讲基本的编程知识，相当不错。
- 在线编程入门的网站：[Codecademy: Learn Python](https://www.codecademy.com/catalog)
- 在线编程练习[CodeAbbey](http://www.codeabbey.com/index/task_list)

## 入门教程
- [MDN的Web开发入门](https://developer.mozilla.org/zh-CN/docs/Learn/Getting_started_with_the_web)
- [Python 编程快速上手](https://book.douban.com/subject/26836700/)
- [Python 编程：从入门到实践](https://book.douban.com/subject/26829016/)
- [MDN JavaScript 教程](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript) 最权威的JavaScript官方教程，从初级到中级再到高级
- [JavaScript 全栈教程](https://www.liaoxuefeng.com/wiki/1022910821149312) 偏应用的教程，也是偏 Web 方面的编程，同时包括涉及后端的 Node.js 方面的教程。
- [W3School JavaScript 教程](https://www.w3cschool.cn/javascript/) 这个教程比较偏 Web 方面的编程
- [Linux教程](https://www.w3cschool.cn/linux/) 操作系统入门Linux 
- **编程工具** Visual Studio Code，这个工具潜力十足，用它开发 Python、JavaScript、Java、Go、C/C++ 都能得心应手 
- **Web 编程入门** 
  - 前端基础：[CSS 文档](https://developer.mozilla.org/zh-CN/docs/Web/CSS)和[HTML 文档](https://developer.mozilla.org/zh-CN/docs/Web/HTML)。理解DOM和动态网页可以参考看 [W3Schools 的 JavaScript HTML DOM 的教程](https://www.w3schools.com/js/js_htmldom.asp)。
  - 后端基础：如果你想省点事，不想再学一门新的语言了，那么你可以直接用 Python 或者 Node.js。也可以搞搞 PHP，它也是很快就可以上手的语言。学习 PHP 语言，你可以先跟着 [W3School 的 PHP 教程](https://www.w3school.com.cn/php/index.asp) 玩玩（其中有连接数据库的 MySQL 的教程）。然后，以 [PHP 的官网文档](https://www.php.net/manual/zh/) 作为更全的文档来学习或查找相关的技术细节。
- **学习要点：**
  - 学习 HTML 基本语法。
  - 学习 CSS 如何选中 HTML 元素并应用一些基本样式。
  - 学会用 Firefox + Firebug 或 Chrome 查看你觉得很炫的网页结构，并动态修改。
  - 在一台 Linux 机器上配置 LEMP - Ubuntu/Nginx/PHP/MySQL 这个环境。
  - 学习 PHP，让后台 PHP 和前台 HTML 进行数据交互，对服务器响应浏览器请求形成初步认识，并实现一个表单提交和反显的功能。
  - 把 PHP 连接本地或者远程数据库 MySQL（MySQL 和 SQL 现学现用够了）。
  - 关于入门的三门后端语言(PHP, Python, Nodejs)：推荐还是主要Python。Node.js 号称 JavaScript 的后端版，但从目前发展来说，在后端的世界里，并不能承担大任，而且问题很多。PHP发展潜力有限。
  - Web 前端编程的感觉，只是为了入门而已。
- **入门实践项目**
  - 无论你用 Python，还是 Node.js，还是 PHP，我希望你能做一个非常简单的 Blog 系统，或是 BBS 系统，需要支持如下功能：
    1. 用户登录和注册（不需密码找回）。
    2. 用户发贴（不需要支持富文本，只需要支持纯文本）。
    3. 用户评论（不需要支持富文本，只需要支持纯文本）。
  - 这里有几个技术点你需要关注一下。
    1. 用户登录时的密码不应该保存为明文，应该用 MD5+Salt 来保存（关于这个是什么，希望你能自行 Google）。
    2. 用户登录后，对于用户自己的贴子可以有“重新编辑”或 “删除”的功能，但是无权编辑或删除其它用户的贴子。
    3. 数据库的设计，你需要三张表：用户表、文章表和评论表，它们之间是怎么关联的，你需要学习一下。这里有个 PHP 的 blog 教你怎么建表，你可以 [前往一读](https://code.tutsplus.com/tutorials/how-to-create-a-phpmysql-powered-forum-from-scratch--net-10188)。
  - 如果你有兴趣，你可以顺着这个小项目，研究一下下面这几个事。
    1. 图片验证码。
    2. 上传图片。
    3. 阻止用户在发文章或评论时输入带 HTML 或 JavaScript 的内容。
    4. 防范 SQL 注入。参看 [PHP 官方文档](https://www.php.net/manual/zh/security.database.sql-injection.php) 或 [微软官方文档](https://learn.microsoft.com/zh-cn/previous-versions/sql/sql-server-2008-r2/ms161953(v=sql.105)?redirectedfrom=MSDN)，或者你自己 Google 一下。
  > 上面这些东西，不是什么高深的东西，但是可以让你从中学到很多。相信你只需要自己 Google 一下就能搞定。

## 正式入门


> 注，以上摘要内容来自极客时间干货满满的专栏：[左耳听风](https://time.geekbang.org/column/intro/100002201?utm_term=discoverbanner-pc&utm_medium=geektime&tab=catalog)