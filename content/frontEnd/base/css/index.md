css
#### 一、
### 二、less和sass它们之间的相同和区别
#### 1、相同点：
- 都是CSS的预编译工具
- 都可以定义变量、函数、继承等
- 各自都有内置的全局函数
#### 2、不同点：
- less定义变量符是:'@'，sass定义变量符是：'$'
- less定义函数混入函数之间写**.fuc(params){}**，在使用时直接.fuc(传参数)；sass定义混入函数用:@mixin fuc(params){},使用时采用@include @fuc(传参数)。
- less继承使用&:extend(继承类名);sass继承使用@extend 继承类名。
### 三、用CSS隐藏页面上的一个元素有那些？
- displya:none
- visibility:hiden
- opacity:0
- position:fixed;left:-999999;top:-9999;设置fixed并设置足够大的负距离left top使其隐藏。
- z-index:-9999,用层叠关系z-index把元素叠在最底下使其隐藏
- 使用text-indent:-9999px使其文字隐藏