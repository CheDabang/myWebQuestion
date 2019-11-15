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

###  解决方案：

#### ①对整个表单或者某个input表格关闭自动填充功能


```
<form autocomplete="off">
<input type="text" autocomplete="off">
```

#### ②给这个input添加阴影

原理利用input阴影直接覆盖Google添加背景色之上

```
 -webkit-box-shadow: 0 0 0px 1000px white inset;
 -webkit-box-shadow: 0 0 0px 1000px rgba(255, 255, 255, 0.1) inset;
```
说明一下，貌似只能用纯色。用rgba制式，浏览器依旧按照rgb颜色进行解析

### ③利用动画关键帧解决

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