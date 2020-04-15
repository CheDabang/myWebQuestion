# 前端问题合集
记叙自己在前端代码之路，遇到一些问题、以及解决过程。

### 问题一：input输入框自动填充，会出现一个高亮样式覆盖原input样式

+ 问题环境： Chrome浏览器版本78

+ 问题排查： 通过检查代F12控制台检查发现, 当代码填充之后多了如下代码

```
input:-internal-autofill-selected {
    background-color: rgb(232, 240, 254) !important;
    background-image: none !important;
    color: rgb(0, 0, 0) !important;
}
```

#### ① 对整个表单或者某个input表格关闭自动填充功能


```
<form autocomplete="off">
<input type="text" autocomplete="off">
```

#### ② 给这个input添加阴影

原理利用input阴影直接覆盖Google添加背景色之上

```
 -webkit-box-shadow: 0 0 0px 1000px white inset;
 -webkit-box-shadow: 0 0 0px 1000px rgba(255, 255, 255, 0.1) inset;
```
说明一下，貌似只能用纯色。用rgba制式，浏览器依旧按照rgb颜色进行解析

#### ③ 利用动画关键帧解决

```
@-webkit-keyframes autofill {
    to {
        color: #666;
        background: transparent;
    }
}

input:-webkit-autofill {
    -webkit-animation-name: autofill;
    -webkit-animation-fill-mode: both;
}
```
解决方法来源:[stackoverflow: removing-input-background-colour-for-chrome-autocomplete](https://stackoverflow.com/questions/2781549/removing-input-background-colour-for-chrome-autocomplete)

### 多行文本垂直居中

#### ① 上下padding书写固定的值，即主体内容自由扩展

#### ② 利用display：table；

```
.center-table {
  display: table;
  
}
.center-table p {
  display: table-cell;
  vertical-align: middle;
}
```
这里父元素就是用的center-table, 然后子元素就是p标签


#### ③ flex布局解决
```
display: flex;
flex-direction: column;
justify-content: center;
```
给包裹文字的盒子写上几个居中属性就ojbk

#### ④ 粗暴解决流

给多行文本包裹一个DIv，然后然后让这个div垂直居中

### css文本处理
`text-indent` 控制文本缩进

`letter-spacing`  用来控制文字之间的间隔

有趣的是，如果单独使用letter-spacing，会导致css另外一个属性
`text-align：center` 出现问题了，这个就需要text-indent写上相同的值，来纠正一下`letter-spacing`导致的bug。
例如：
```
text-align: center;
text-indent: px2rem(100);
letter-spacing: px2rem(100);
```

### 移动端a标签去除默认淡蓝色阴影
移动端我们在点击页面中的一些图片的时候会出现阴影。处理方法只要给a标签加上
```
a{
    -webkit-tap-highlight-color: transparent;
    -webkit-touch-callout: none;
    -webkit-user-select: none;
}    
```
理论第一行代码就ok了，将tap情况的高亮色给设置成透明色

### 移动端禁止长按进行复制粘贴
```
*{
	-webkit-touch-callout:none; /系统默认菜单被禁用/
	-webkit-user-select:none; /webkit浏览器/
	-moz-user-select:none;/火狐/
	-ms-user-select:none; /IE10/
	user-select:none;
}
// 但是这么搞，IOS会有问题。IOS输入框自动失去焦点无法输入内容。得加入如下的玩意
input , textarea{
	-webkit-user-select:auto;
}
```

### css文本省略号

+ 单行省略号
```
overflow: hidden;
text-overflow:ellipsis;
white-space: nowrap;
```
+ 多行省略号
```
display: -webkit-box;
-webkit-box-orient: vertical;
-webkit-line-clamp: 3;
overflow: hidden;
```
### flex不能自动撑开父盒子
需求： 想利用flex横着排列，将父盒子的宽度给撑开. 但是父盒子总是自动调整子盒子宽度。

给子盒子添加: `flex-shrink: 0`, 子盒子就不会受父盒子影响, `flex-shrink`默认值是`1`
