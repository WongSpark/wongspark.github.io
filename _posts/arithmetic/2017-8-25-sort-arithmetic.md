---
layout: post
title: 排序算法实践
categories: arithmetic
description: 一次关于排序算法的实践
keywords: 排序，算法
---

# 背景
数据结构和算法是程序员绕不过的坎儿。这两项是程序员的基本功，也是最考验编程思想的地方。单从概念上来讲，学习他们并不非常困难，真正的难处在于在编码过程中理解它、使用它。

# 冒泡排序

## 算法简述

冒泡排序的基本思路是将前后两个元素进行比较，较大的后移，较小的前置。这样，一次排序下来，最大的元素就被移动到了数组的最后一位，依次按以上逻辑循环。
因此，冒泡排序的时间复杂度的最好情况是O（n），此时，数组本来就是正序排列，不需要排序，哈哈。
最坏情况是O（n*n），此时，数组是严格的倒叙排列。
平均时间复杂度是O（n*n）
以下为冒泡排序的动图演示。

![import lib](/images/arithmetic/bubble.gif)

## 算法具体描述

具体算法描述如下：

<1>.比较相邻的元素。如果第一个比第二个大，就交换它们两个；
<2>.对每一对相邻元素作同样的工作，从开始第一对到结尾的最后一对，这样在最后的元素应该会是最大的数；
<3>.针对所有的元素重复以上的步骤，除了最后一个；
<4>.重复步骤1~3，直到排序完成。

## 算法实现

```javascript
function bubbleSort(arr) {
    var len = arr.length;
    for (var i = 0; i < len; i++) {
        for (var j = 0; j < len - 1 - i; j++) {
            if (arr[j] > arr[j+1]) {        //相邻元素两两对比
                var temp = arr[j+1];        //元素交换
                arr[j+1] = arr[j];
                arr[j] = temp;
            }
        }
    }
    return arr;
}
var arr=[3,44,38,5,47,15,36,26,27,2,46,4,19,50,48];
console.log(bubbleSort(arr));
```
`2017-8-27补充`

--冒泡排序优化1

以上冒泡排序有可以改进的地方：冒泡排序每一次比较的目的都是为了找出最大值，将其放置到数组最后一位，然后下一次循环比较时，只比较前一次循环长度减一次，最后达到从小到大排列的顺序。
假设某次循环的后半部分本来就是按照正序排列的，那么我们其实完全没有必要再进行比较，只需记录下最后一次交换数组元素的位置，比较其前面的元素大小即可（没有交换元素说明后面的元素都是按正序排列的），因此可以对冒泡排序进行优化，如下：

```javascript
function bubbleSort2(arr) {
    console.time('改进后冒泡排序耗时');
    var i = arr.length-1;  //初始时,最后位置保持不变
    while ( i> 0) {
        var pos= 0; //每趟开始时,无记录交换
        for (var j= 0; j< i; j++)
            if (arr[j]> arr[j+1]) {
                pos= j; //记录交换的位置
                var tmp = arr[j]; arr[j]=arr[j+1];arr[j+1]=tmp;
            }
        i= pos; //为下一趟排序作准备
     }
     console.timeEnd('改进后冒泡排序耗时');
     return arr;
}
var arr=[3,44,38,5,47,15,36,26,27,2,46,4,19,50,48];
console.log(bubbleSort2(arr));
```

--冒泡排序优化2

传统冒泡排序中每一趟排序操作只能找到一个最大值或最小值,我们考虑利用在每趟排序中进行正向和反向两遍冒泡的方法一次可以得到两个最终值(最大者和最小者) , 从而使排序趟数几乎减少了一半。

```javascript
function bubbleSort3(arr3) {
    var low = 0;
    var high= arr.length-1; //设置变量的初始值
    var tmp,j;
    console.time('2.改进后冒泡排序耗时');
    while (low < high) {
        for (j= low; j< high; ++j) //正向冒泡,找到最大者
            if (arr[j]> arr[j+1]) {
                tmp = arr[j]; arr[j]=arr[j+1];arr[j+1]=tmp;
            }
        --high;                 //修改high值, 前移一位
        for (j=high; j>low; --j) //反向冒泡,找到最小者
            if (arr[j]<arr[j-1]) {
                tmp = arr[j]; arr[j]=arr[j-1];arr[j-1]=tmp;
            }
        ++low;                  //修改low值,后移一位
    }
    console.timeEnd('2.改进后冒泡排序耗时');
    return arr3;
}
var arr=[3,44,38,5,47,15,36,26,27,2,46,4,19,50,48];
console.log(bubbleSort3(arr));
```

#选择排序

##算法简介

选择排序是所有排序算法中最容易想到的。其基本思路就是循环比较所有的元素，每一次都找到数组中最小的元素，放置到队首，然后再从未排序的元素中找到最小的，放置到第二位，依次循环，直至排序完成。

![import lib](/images/arithmetic/selection.gif)

##代码实现

```javascript
function selectionSort(arr) {
    var len = arr.length;
    var minIndex, temp;
    console.time('选择排序耗时');
    for (var i = 0; i < len - 1; i++) {
        minIndex = i;
        for (var j = i + 1; j < len; j++) {
            if (arr[j] < arr[minIndex]) {     //寻找最小的数
                minIndex = j;                 //将最小数的索引保存
            }
        }
        temp = arr[i];
        arr[i] = arr[minIndex];
        arr[minIndex] = temp;
    }
    console.timeEnd('选择排序耗时');
    return arr;
}
var arr=[3,44,38,5,47,15,36,26,27,2,46,4,19,50,48];
console.log(selectionSort(arr));
```



# 结束语
一个个算法来吧，后续补充
