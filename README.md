# 前端问题合集
记叙自己在前端代码之路，遇到一些问题、以及解决过程。

# 问题一：input输入框自动填充，会出现一个高亮样式覆盖原input样式

## 问题环境： Chrome浏览器版本78

## 问题排查： 通过检查代F12控制台检查发现, 当代码填充之后多了如下代码

```
input:-internal-autofill-selected {
    background-color: rgb(232, 240, 254) !important;
    background-image: none !important;
    color: rgb(0, 0, 0) !important;
}
```

## 解决方案：

### 方案一

对整个表单或者某个input表格关闭自动填充功能
```
<form autocomplete="off">
<input type="text" autocomplete="off">
```

### 给这个input添加阴影

原理利用input阴影直接覆盖Google添加背景色之上

```
 -webkit-box-shadow: 0 0 0px 1000px white inset;
 -webkit-box-shadow: 0 0 0px 1000px rgba(255, 255, 255, 0.1) inset;
```
说明一下，貌似只能用纯色。用rgba制式，浏览器依旧按照rgb颜色进行解析

### 利用动画关键帧解决

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