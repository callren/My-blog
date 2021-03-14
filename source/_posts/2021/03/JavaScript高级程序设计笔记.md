---
title: JavaScript高级程序设计笔记
date: 2021-03-14 16:51:43
tags: 红宝书
categories: 红宝书
---

# HTML中的JavaScript

## script元素属性

- async：立即开始下载脚本，不能阻止页面其它动作
- charset：指定代码字符集
- crossorigin：配置相关请求的cors设置，也就是跨域资源共享
- defer：在文档解析和显示完成后再执行脚本是没有问题的
- integrity：允许对比接收到的资源和指定的加密签名以验证子资源完整性
- src：要执行代码的外部文件
- type：代码块中脚本语言的内容类型，一般为'text/javascript'，如果为'module'，则代码为es6模块，只有这个时候代码中才能够出现import和export关键字

一般来说代码块会被保存在解释器环境中，并且js代码在被解释的过程中，页面的其余内容不会被加载也不会被展示。在行内的js代码中，代码不能出现`</script>`字符串，否则会报错，需要转义字符`\`
