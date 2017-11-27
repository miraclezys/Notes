# Web优化

## dns-prefetch

例子：

```html
<link rel="dns-prefetch" href="//g.alicdn.com">
```

dns-prefetch是指对DNS的与读取，即浏览器在后台执行DNS预读取功能，那么DNS很可能在访问链接前就已经解析完毕了，从而减少用户点击链接是的延迟。