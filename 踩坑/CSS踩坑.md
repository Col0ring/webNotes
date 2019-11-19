# CSS踩坑

## 1.flex布局相关

### 1.1 子元素宽度问题

flex布局下子元素设置`flex:1`，在正常情况下占满剩下的空间，但是当该子元素的子元素超出范围时会将其宽度撑开，这时我们需要给改子元素加上`width:0`就会和正常情况下占满剩余空间一样

```css
.flex-child{
    flex:1;
    width:0;
}
```



### 1.2 子元素文本截断为省略号

在flex的子元素中如果想要让文本出现截断效果，除了正常的阶段文本效果：

```css
.text-cut{
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
}
```

还需要主动添加一个`display:block`的属性才会显示省略号，不然只能是截断效果

```css
.text-cut{
 	display:block;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
}
```



## 2.transform相关

### 2.1 translate与translate3d的冲突

当使用了`translate3d`后再对同样的元素使用`translate`不会生效，不过我们可以使用：

```css
.demo{
    position:relative;
    left:0;
}
```

这样定位的方法来实现变化效果



## 2.其他

### 2.1 计算函数

CSS中的计算函数用法与JS的是不同的，在计算符号的左右必须用空格隔开要计算的数，不然计算函数式不会生效的，如`calc(100% - 50px)`，`-`号左右需要用空格隔开来