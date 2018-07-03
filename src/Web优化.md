# Web优化

## dns-prefetch

例子：

```html
<link rel="dns-prefetch" href="//g.alicdn.com">
```

dns-prefetch是一种DNS预解析技术。即浏览器会在加载网页时对网页中的域名进行解析缓存，当你点击当前网页中的链接时无需进行DNS解析，减少了用户等待时间，提高用户体验。

目前大多数浏览器已支持该属性，其中 Chrome 和 Firefox 3.5+ 内置了 DNS Prefetching 技术并对DNS预解析做了相应优化设置。 