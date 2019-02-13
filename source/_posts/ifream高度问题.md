---
title: ifream高度问题
date: 2018-10-31 15:21:00
tags: 
categories: 前端
top:
---

Add this to your `<head>` section:

```html
<script>
  function resizeIframe(obj) {
    obj.style.height = obj.contentWindow.document.body.scrollHeight + 'px';
  }
</script>
```

And change your iframe to this:

```html
<iframe src="..." frameborder="0" scrolling="no" onload="resizeIframe(this)" />
```

As found on [sitepoint discussion](https://www.sitepoint.com/community/t/auto-height-iframe-content-script/67843).