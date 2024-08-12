## xss攻击

当后端直接返回代码，需要前端进行渲染的时候，如果对返回代码进行截断修改，如返回

<a onload='javascript:console.log(document.cookie)'>大紧急<a> 就会将用户的cookie信息泄露。

借助dompurify包进行过滤

```
npm i dompurify

import Dompurnify

Dompurify.sanitize('<a>点击<a>')  //就会对非法xss攻击进行去除，返回合法的字符串

```

