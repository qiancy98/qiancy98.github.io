---
layout: post
title: "命令行：“libpng warning: iCCP: known incorrect sRGB profile”问题解析"
---

我之前在命令行中数次看到如下报错：

```text
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: cHRM chunk does not match sRGB
```

两个参考网站：

- [libpng warning: iCCP: known incorrect sRGB profile - Stack Overflow](https://stackoverflow.com/questions/22745076/libpng-warning-iccp-known-incorrect-srgb-profile)
- [有关libpng warning: iCCP: known incorrect sRGB profile警告的原因 - 萌梦社区](https://qtdream.com/topic/774/%E6%9C%89%E5%85%B3libpng-warning-iccp-known-incorrect-srgb-profile%E8%AD%A6%E5%91%8A%E7%9A%84%E5%8E%9F%E5%9B%A0)

> libpng 1.6及以上版本对PNG图片的字段检查更加严格，诸如PhotoShop或者GIMP处理图片时“模式”选择不对就会出现这个警告
>
> PNG图片的原始文件由8个字节的文件标识，4个标准(关键)数据块（必须包含），16个辅助数据块（可选包含）组成
> Screenshot from 2016-11-03 11-37-37.png
> 出问题的是16个辅助数据块中的iCCP数据块和sRGB数据块，这两个都是设置图片颜色模式的数据块，而且是“或”的关系，也就是每个PNG图片如果包含那就是能是“二选一”.

所以总结就是，这是使用不规范的PNG格式文件所致，对于一般程序不用理会就好。
