---
title: 避免Jekyll模板解析引擎(Liquid)的解析冲突
---

有的时候需要在页面中使用一些类似{% raw %}`{{` `}}`和`{%` `%}`{% endraw %}这样的模板标签(比如[这里]({% post_url 2017-02-15-vue-component-demo %}))，然而这会造成Liquid的解析冲突。

解决方法是用{<span></span>% raw %<span></span>} 和 {<span></span>% endraw %<span></span>}标签，包围在不需要解析的内容前后即可。

PS：那么上面最后一句话到底要怎么才能正确输出啊喂_(:з」∠)_