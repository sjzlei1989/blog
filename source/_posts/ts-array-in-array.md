---
title: Typescript判断一个二维数组中是否包含一个一维数组
tags: [ts,js,javascript,typescript,数组]
categories: [typescript]
date: 2018-12-27 16:39:58
---

项目需要判断一个二维数组中是否包含一个一维数组
```typescript
let a = [[0,0],[1,0],[2,0],[3,0],[4,0],[4,1],[4,2],[4,3],[4,4]];
let b = [1,0];
```
因为以前用C#, 所以首先想到的是contains方法, 没想到ts的数组没有这个方法.
<!--more-->
后来使用indexOf方法, 如果包含指定元素则返回索引值, 不包含则返回-1. 但是试了一下发现一直返回-1
```typescript
a.indexOf(b);
a.indexOf([1,0]);
a.indexOf(new Array(1,0));
```
无奈求助于Google, 找到segmentfault上的一个回答才明白是怎么回事. [链接](https://segmentfault.com/q/1010000005826627/)
```
对象的数组并不是不能使用 indexOf ，来判断对象在数组的位置。
arr.indexOf({name:"Alex"}) 只是你上面的写法 是让数组 去判断一个 新创建的对象，
所以会得到-1
```

于是自己动手写了一个小函数来实现这个功能
```typescript
    /**
    * 一个二维数组中是否包含一个一维数组
    * @param array 
    * @param element 
    */
    static arrayHasElement(array: any[][], element: any[]): boolean {
        for(let arr of array) {
            //如果长度不同则肯定不同, 直接进入下一个循环
            if(arr.length != element.length) continue;
            //长度相同的话遍历两个数组中的元素判断是否相等
            for(let index in element) {
                //如果两个元素相等, 并且索引值已经是最后一个, 则认为这两个数组相等
                if(arr[index] == element[index]) {
                    if(parseInt(index) == element.length - 1) return true;
                }
                //如果两个元素不等, 跳出for循环
                else break;
            }
        }
        return false;
    }
```

