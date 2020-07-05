### 1、设置在线可编写
在浏览器地址中输入代码,并回车即可把浏览器临时编辑器
```js
data:text/html,<html contenteditable>
```
【ctrl+shift+J】调用javascript控制台，在控制台输入：document.body.contentEditable = true,就可以编辑当前网页了。
### 2、计算字符长度，包括中文
```js
//首先把中文替换成两个字节的英文，在计算长度
str.replace(/[\u0391-\uFFE5]/g,'aa').length 
```
