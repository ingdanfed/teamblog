---
title: Umeditor
date: 2017-06-08 10:22:58
tags: 
- umeditor
- 填坑
---
在IE下，点击富文本编辑器的图片，会自动打开文件浏览器

<!-- more -->
使用简易的ueditor，即umeditor生成的富文本编辑器，如下：


![image](/images/umeditor_01.png)

点击上图的图片，会弹出一个上传图片的框，如下：

![image](/images/umeditor_02.png)


那么在IE下，打开图二的同时，会自动打开文件浏览器，然后关闭文件浏览器，会发现两点：
1. 上传图片框里面的input[type=file]会自动获取焦点
2. 点击富文本编辑器和上传图片框的任何区域，都会自动触发input[type=file]的点击事件自动打开文件浏览器


**原因**： **使用了label标签包裹**

```
<label class="in-box in-box-editor">
    <span class="name">工作计划</span>
    <div class="editor-box">
        {{# var contentEditor = '<script id="contentEditor" name="content" type="text\/plain"><\/script>'; }}
        {{ contentEditor }}
    </div>
 </label>
```



**解决办法**：**改成div标签包裹即可**



```
<div class="in-box in-box-editor">
    <span class="name">工作计划</span>
    <div class="editor-box">
        {{# var contentEditor = '<script id="contentEditor" name="content" type="text\/plain"><\/script>'; }}
        {{ contentEditor }}
    </div>
</div>
```



