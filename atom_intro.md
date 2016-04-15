# 如何评价 GitHub 发布的文本编辑器 Atom？

今天拿到邀请试用了一会儿，可以明确的说跟 Sublime 没有关系。

Sublime 是原生界面，脚本用的是 python；

Atom 应该是基于 Chromium Embedded Framework，基本上就是个 web app，

源码都是 CoffeeScript 写的，连界面都可以用 CSS 来自定义。

除了基本的操作和界面外，和 Sublime 最大的差别在于扩展性。

Atom 非常强调模块化，很多默认功能也都是开源的模块。

自带友好的模块管理界面，相比之下 Sublime 需要自己手动安装，或是依赖第三方的 package control。

Atom 的扩展也是用 JS 或者 Coffee 在 Node + webkit 的环境下开发，并且可以使用 npm 的包，

这对于前端和 Node 开发者是很有诱惑力的，需要的话完全可以把 Atom 打造成一个 IDE。

一个明显的缺点是，启动和开文件速度明显不如 Sublime 3。

GitHub 创始人 Mojombo 在论坛上说 Atom 正式发布以后是要收费的，内核将会是以限制性的协议开源，可以看了学习，但是不能拿来商用。

其他所有官方模块都是 MIT 协议开源。

从策略上来讲，GitHub 以后肯定会通过官方模块把 Atom 和 GitHub 进行深度整合。

收费我估计不会贵到哪里去，说到底让开发者因为 Atom 而用 GitHub 用得爽，进一步加强用户黏度才是目的吧。

这和 Google 做浏览器是一个道理。

- - - -

补充一些技术实现上的细节：打开 Package Content 看了下，可以看到内部的由 CoffeeScript 编译过来的 JavaScript。

内部的一些模块在 GitHub 上也能找到源码。

核心的两个库，一个是界面库 [Space Pen](https://github.com/atom/space-pen)，自带创建 `DOM` 的 `DSL，并继承自` `jQuery` 原型，可以看做加强版的 `Backbone.View`。

另一个库是 [Theorist](https://github.com/atom/theorist)，一个基于事件的模型库，类似 `Backbone.Model` + `Backbone.Collection`。

内部用了大量的 `emitter` 和 `subscriber` 模式。总的来说，代码很清晰，模块化做得非常好，非常值得做 SPA 的前端学习。

 * 作者：尤雨溪
 * 链接：https://www.zhihu.com/question/22867204/answer/22944645
 * 来源：知乎
 * 著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
