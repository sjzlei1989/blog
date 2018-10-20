---
title: Typescript生成随机数
tags: [ts, typescript, random, 随机数]
categories: [typescript]
date: 2018-07-18 17:00:12
---
直接上代码，注释里写的挺清楚的了。
<!--more-->
其中Math.floar(num)方法会返回一个小于等于传入值num的整数。
```typescript
/**
 * 获取一个范围内的随机数
 * @param min 最小值(包括)
 * @param max 最大值(不包括)
 */
public static getRandomInteger(min: number, max: number): number {
    if(min == max) {
        return min;
    }
    if(min > max) {
        return Util.getRandomInteger(max, min);
    }
    let range = max - min;
    let rand = Math.random();
    return (min + Math.floor(rand * range));
}
```

