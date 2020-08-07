---
title: JavaScript 开发
date: 2020-07-26 16:19:11
tags: basic
categories: JavaScript
---

# 判断设备

1. 判断当前设备类型

```javascript

  getPhoneType() {
    let result = false
    const u = navigator.userAgent
    if (u.indexOf('Android') > -1 || u.indexOf('Linux') > -1) { // 安卓手机
      result = false
    } else if (u.indexOf('iPhone') > -1) { // 苹果手机
      result = true
    } else if (u.indexOf('Windows Phone') > -1) { // winphone手机
      result = false
    } else if (u.indexOf('iPhone Simulator') > -1) {
      result = true
    }
    return result
  }

```

2. 判断是否 IPhoneX 系列

```javascript

isIphoneXall() {
  // iPhone X、iPhone XS
  const isIPhoneX = /iphone/gi.test(window.navigator.userAgent) && window.screen.width === 375 && window.screen.height === 812
  // iPhone XS Max
  const isIPhoneXSMax = /iphone/gi.test(window.navigator.userAgent) && window.screen.width === 414 && window.screen.height === 896
  // iPhone XR
  const isIPhoneXR = /iphone/gi.test(window.navigator.userAgent) && window.screen.width === 414 && window.screen.height === 896
  if (isIPhoneX || isIPhoneXSMax || isIPhoneXR) {
    return true
  }
  return false    
}

```

# 判断是否是 url

```javascript

const isUrl = str => /^(((ht|f)tps?):\/\/)?[\w-]+(\.[\w-]+)+([\w.,@?^=%&:/~+#-]*[\w@?^=%&/~+#-])?$/.test(str)
// e.g.
isUrl('https://www.baidu.com') // true
isUrl('https://www') // false

```

# 连字符和驼峰互相转换

1. 连字符转驼峰

```javascript

const toCamelCase = (str = '', separator = '-') => {
    if (typeof str !== 'string') {
        throw new Error('Argument must be a string')
    }
    if (str === '') {
        return str
    }
    const newExp = new RegExp('\\-\(\\w\)', 'g')
    return str.replace(newExp, (matched, $1) => {
        return $1.toUpperCase()
    })
}

// e.g.
toCamelCase('hello-world') // helloWorld

```

2. 驼峰转连字符

```javascript

const fromCamelCase = (str = '', separator = '-') => {
    if (typeof str !== 'string') {
        throw new Error('Argument must be a string')
    }
    if (str === '') {
        return str
    }
    return str.replace(/([A-Z])/g, `${separator}$1`).toLowerCase()
}

// e.g.
fromCamelCase('helloWorld') // hello-world

```






# 参考文章

(【适合收藏】为了多点时间陪女朋友，我向BAT大佬跪求了这15条JS技巧)[https://juejin.im/post/6854573211955560455]