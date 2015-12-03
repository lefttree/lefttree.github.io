---
layout: post
title: Bootstrap modal plugin
cover: cover.jpg
category: web
tags: [bootstrap, frontend]
---

翻译自[w3schools](http://www.w3schools.com/bootstrap/bootstrap_modal.asp)

<!-- Trigger the modal with a button -->
<button type="button" class="btn btn-info btn-lg" data-toggle="modal" data-target="#myModal">Open Modal</button>

<!-- Modal -->
<div id="myModal" class="modal fade" role="dialog">
  <div class="modal-dialog">

    <!-- Modal content-->
    <div class="modal-content">
      <div class="modal-header">
        <button type="button" class="close" data-dismiss="modal">&times;</button>
        <h4 class="modal-title">Modal Header</h4>
      </div>
      <div class="modal-body">
        <p>Some text in the modal.</p>
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-default" data-dismiss="modal">Close</button>
      </div>
    </div>

  </div>
</div>

### "触发"部分

触发模态框，需要一个按钮或者链接。

然后要有两个`data-*`属性:

- `data-toggle="modal"` 打开模态框
- `data-target="#myModal"` 指向模态框的id

### 模态框部分

模态框上一层的`<div>`必须要有一个和`data-target`中一样的id，这样才能匹配。

- `.modal`类指定了这个`<div>`里的内容是模态框，把屏幕焦点带到这个模态框上。
- `.fade`类给这个模态框进入和切出时加了一个转换效果。
- `role="dialog"`让视力不好使用screen reader的人更好用
- `.modal-dialog`类为这个`<div>`设置合适的大小和周围边界

### 模态框内容

- 类为`class="modal-content"`的`<div>`为模态框设置样式，它的里面包括头部，内容和脚注
- `.modal-header`
- `.modal-body`
- `.modal-footer`
