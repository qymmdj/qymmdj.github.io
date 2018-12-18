---
layout: post
sidebar: common
title: 常量（const.js）
sidenav: const
---
常量类中包括普通常量（const）、正则常量（regex）。

# 一、普通常量（const）
普通常量中包括cookie、menu、分享回调中设置的TO、JSBridge等常量。

##### **1）分享回调中设置的TO**
```javascript
common.const = {
  TO_WX_F: 'wxMessage',
  TO_WX_T: 'wxTimeline',
  TO_CL_F: 'clMessage',
  TO_QQ: 'qq',
  TO_SINA: 'sina',
  TO_SMS: 'sms',
};
```

# 二、正则常量（regex）
```javascript
common.regex = {
  mobile: /^1[0-9]{10}$/, //手机号码
  chinese: /^[\u4E00-\u9FA5]+$/, //全部是中文
  card: /(^\d{15}$)|(^\d{18}$)|(^\d{17}(\d|X)$)/, //简单的身份证
  email: /^[a-zA-Z0-9.!#$%&'*+\/=?^_`{|}~-]+@[a-zA-Z0-9](?:[a-zA-Z0-9-]{0,61}[a-zA-Z0-9])?(?:\.[a-zA-Z0-9](?:[a-zA-Z0-9-]{0,61}[a-zA-Z0-9])?)*$/, //邮箱
  digits: /^\d+$/ //整数
};
```