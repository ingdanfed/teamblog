---
title: JavaScript排序算法及性能比较
date: 2017-03-30 10:23:43
tags:
categories:
---

作者原文：http://www.boatsky.com/blog/10.html

说到算法，对很多同学来说，启蒙算法应该就是冒泡排序，我们也会见到它解决排序问题，然后，数据量稍大时，它就显的无力了。前端可能用到算法的机会不算多，但用到时又不会，临时去学，就有些痛苦了。

算法很大程度是数学问题，而算法的作者们往往是一些科学家或大牛，这确实需要良好的数学功底。但不要被它吓到，站在巨人的肩膀上，花些时间理解已有理论，就可以直接或间接使用现成算法解决问题，人类的智慧成长不正是代代智慧的叠加么？所以私以为学习算法，并不需要多好的数学素养。

<!-- more -->

前言

说到算法，对很多同学来说，启蒙算法应该就是冒泡排序，我们也会见到它解决排序问题，然后，数据量稍大时，它就显的无力了。前端可能用到算法的机会不算多，但用到时又不会，临时去学，就有些痛苦了。

算法很大程度是数学问题，而算法的作者们往往是一些科学家或大牛，这确实需要良好的数学功底。但不要被它吓到，站在巨人的肩膀上，花些时间理解已有理论，就可以直接或间接使用现成算法解决问题，人类的智慧成长不正是代代智慧的叠加么？所以私以为学习算法，并不需要多好的数学素养。

算法一般都是C/C++或者伪代码讲解，本文则使用JavaScript翻译一遍，大前端时代嘛。本文参考了《数据结构与算法JavaScript描述》。

1.冒泡排序
冒泡排序是最简单的排序算法，效率也是最低的，它把相邻元素两两对比，比较n轮，每轮len-n次，以下都以数组5,1,7,0,9,2,3,8,4,6从小到大排序为例
5,1,7,0,9,2,3,8,4,6
1,5,7,0,9,2,3,8,4,6   1轮,1次，比较5,1的结果
1,5,7,0,9,2,3,8,4,6   1轮,2次，比较5,7的结果
1,5,0,7,9,2,3,8,4,6 1轮,3次，比较7,0的结果
1,5,0,7,9,2,3,8,4,6   1轮,4次，比较7,9的结果
……
1,5,0,7,2,3,8,4,9,6   1轮,8次，比较9,4的结果
1,5,0,7,2,3,8,4,6,9   1轮,9次，比较9,6的结果
第1轮完成，把最大值9移至最后，第2轮开始，我们只需比较8次

1,5,0,7,2,3,8,4,6,9   2轮,1次，比较1,5
1,0,5,7,2,3,8,4,6,9   2轮,2次，比较5,0
1,0,5,7,2,3,8,4,6,9   2轮,3次，比较5,7
1,0,5,2,7,3,8,4,6,9   2轮,4次，比较7,2
……
重复以上操作，第2轮则把8移至第8位，同理经过n(n+1)/2次比较后，得到最终结果。
代码如下：
```javascript
//冒泡排序
function bubbleSort(arr){
    if(arr.length > 1){
        var len = arr.length;
        var i,j,temp;
        for(i = 0;i < len;i++){
            for(j = 0;j < len - i;j++){
                if(arr[j] > arr[j+1]){
                    temp = arr[j];
                    arr[j] = arr[j+1];
                    arr[j+1] = temp;
                }
            }
        }
    }
    return arr;
}
```


2.选择排序
选择排序与冒泡排序有点相似之次，就是都需要比较n(n+1)/2次，不过它是把最小的数字，排在最前面，第2小的数字，排在第2位，虽然比较次数与冒泡排序一样，但数据交换次却常常比冒泡排序少很多，所以它的效率比冒泡排序更高一点。演示：
5,1,7,0,9,2,3,8,4,6
1,5,7,0,9,2,3,8,4,6 1轮,1次，比较5,1
1,5,7,0,9,2,3,8,4,6 1轮,2次，比较1,7
0,5,7,1,9,2,3,8,4,6 1轮,3次，比较1,0
……
0,5,7,1,9,2,3,8,4,6 1轮,9次，比较0,6
第1轮结束，注意了，第一轮第3次之后虽然会一直比较下去，直到0与6比较，但是0已经是最小的数字了，所以之后比较，都无需进行数据交换，提高了效率，但这是不稳定的。

0,5,7,1,9,2,3,8,4,6 2轮,1次，比较5,7
0,1,7,5,9,2,3,8,4,6 2轮,2次，比较5,1
……
0,1,7,5,9,2,3,8,4,6 2轮,8次，比较1,6
重复n-1轮，得出最终结果。

```javascript
//选择排序
function selectSort(arr){
    if(arr.length > 1){
        var len = arr.length;
        var i,j,temp;
        for(i = 0;i < len;i++){
            for(j = i+1;j < len;j++){
                if(arr[i] > arr[j]){
                    temp = arr[i];
                    arr[i] = arr[j];
                    arr[j] = temp;
                }
            }
        }
    }
    return arr;
}
```



3.插入排序
插入排序，是把数据一个插入已排序的数组中，只需插入n-1遍。
5,1,7,0,9,2,3,8,4,6
1,5,7,0,9,2,3,8,4,6    1插入[5]中结果
1,5,7,0,9,2,3,8,4,6    7插入[1,5]中结果
0,1,5,7,9,2,3,8,4,6    0插入[1,5,7]中结果
0,1,5,7,9,2,3,8,4,6    9插入[0,1,5,7]中结果
……
0,1,2,3,4,5,7,8,9,6    4插入[0,1,2,3,5,7,8,9,6]中结果
0,1,2,3,4,5,6,7,8,9    6插入[0,1,2,3,4,5,6,7,8,9]中结果
一共比较n(n+1)/2次，最坏的情况也需要交换这么多次数据，最好的情况，一次数据交换也不需要，所以它是不稳定的，但一般比选择排序快上一些。

代码如下：
```javascript
function insertSort(arr){
    if(arr.length > 1){
        var len = arr.length;
        var i,j,temp;
        for(i = 1;i < len;i++){
            temp = arr[i];
            j = i;
            while(j > 0 && arr[j-1] > temp){
                arr[j] = arr[j-1];
                j--;
            }
            arr[j] = temp;
        }
    }
    return arr;
}
```


4.归并排序
归并排序，则是把已经排好序的子数组合并成一个大的数组。
5,1,7,0,9,2,3,8,4,6
5,  1,  7,  0,  9,  2,  3,  8,  4,  6
step 1
1,5,  0,7,  2,9,  3,8  4,6
step 2
0,1,5,7,  2,3,8,9,  4,6
step 4
0,1,2,3,5,7,8,9,  4,6

最终
0,1,2,3,4,5,6,7,8,9

归并排序相对稳定，并且占用内存少，效率一般也比插入排序高。如数据量巨大，不方便使用太占内存的算法时，归并排序是你的选择！
代码如下：
```javascript
function mergeSort(arr){
    if (arr.length < 2){
        return arr;
    }
    var step = 1;
    var left, right;
    while(step < arr.length){
        left = 0;
        right = step;
        while(right + step <= arr.length){
            mergeArrays(arr, left, left+step, right, right+step);
            left = right + step;
            right = left + step;
        }
        if (right < arr.length){
            mergeArrays(arr, left, left+step, right, arr.length);
        }
        step *= 2;
    }
    return arr;
}
function mergeArrays(arr, startLeft, stopLeft, startRight, stopRight){
    var leftArr = new Array(stopLeft - startLeft + 1);
    var rightArr = new Array(stopRight - startRight + 1);
    var k = startLeft;
    for(var i = 0;i < (leftArr.length-1);i++){
        leftArr[i] = arr[k];
        k++;
    }
    k = startRight;
    for(var i = 0;i < (rightArr.length-1);i++){
        rightArr[i] = arr[k];
        k++;
    }
    rightArr[rightArr.length-1] = Infinity;
    leftArr[leftArr.length-1] = Infinity;
    var m = 0,n = 0;
    for(k = startLeft;k < stopRight;k++){
        if (leftArr[m] <= rightArr[n]){
            arr[k] = leftArr[m];
            m++;
        }
        else {
            arr[k] = rightArr[n];
            n++;
        }
    }
}
```


5.希尔排序
希尔排序，又是插入排序的改良版，即分组插入。
5,1,7,0,9,2,3,8,4,6
g为5时，分成5组
5与2比较，1与3比较，7与8比较，0与4比较，9与6比较
2,1,7,0,6,5,3,8,4,9
g为2时，分成2组
2,7,6,3,4与1,0,5,8,9，分别进行插入排序得到2,3,4,6,7与0,1,5,8,9
结果为
2,0,3,1,4,5,6,8,9,9
g为1时，进行插入排序得到
0,1,2,3,4,5,6,8,9,7,8
其并不稳定，但因为使用间隔比较，减少了交换次数，在多数情况，比归并更快一点点。
代码如下：
```javascript
//希尔排序
function shellSort(arr){
    if(arr.length > 1){
        var len = arr.length;
        var g,i,j, k,temp;
        //第几轮分组
        for(g = Math.floor(len / 2);g > 0;g = Math.floor(g / 2)){
            for(i = 0;i < g;i++){
                for(j = i + g;j < len;j = j + g){
                    if(arr[j - g] > arr[j]){
                        temp = arr[j];
                        k = j - g;
                        while(k >= 0 && arr[k] > temp){
                            arr[k + g] = arr[k];
                            k = k - g;
                        }
                        arr[k + g] = temp;
                    }
                }
            }
        }

    }
    return arr;
}
```

6.快速排序
快速排序即排序很快速！那为什么它这么快？
官方的描述是：通过一趟排序将要排序的数据分割成独立的两部分，其中一部分的所有数据都比另外一部分的所有数据都要小，然后再按此方法对这两部分数据分别进行快速排序，整个排序过程可以递归进行，以此达到整个数据变成有序序列。
我们用实例来演示一下：
5,1,7,0,9,2,3,8,4,6
选中第0个元素即5作为中间值(不是大小的中间值，只是确定一个固定比较对象)，比5小的放在左数组，比5大的放在右数组，则变成
[1,0,2,3,4]+5+[7,9,8,6]
与此同时(不是第二步，整个过程都是在1步完成的)，选中左、右数组的第1个元素作为中间值。(ps: 为了好看，我把中间写成是一个数字，其实它是一个单元素数组)
[0] + 1 + [2,3,4] + 5 + [6] + 7 + [9,8]
与此同时再分解
[0] + 1 +  2 + [3,4] + 5 + [6] + 7 + 9 + [8]
同时
[0] + 1 +  2 +  3 + [4] + 5 + [6] + 7 + 9 + [8]
快的原因就是它使用空间换取时间，不断回调自己。如果数据量太大，其实是不建议这么干的。
后面我做了一个小数据量（即10000）的情况下，内存基本没什么影响，使用不同的数据，计算了100次，算出其效率比希尔排序还要快10倍，比冒泡排序快4698倍，数据量越大，其占内存越大，同时，其与其他排序方式的速度差距也越大。
```javascript
//快速排序
function quickSort(arr) {
    var len = arr.length;
    if(len == 0){
        return [];
    }
    else if(len == 1){
        return arr;
    }
    var smallArr = [];
    var largeArr = [];
    var pivot = arr[0];
    for (var i = 1; i < len; i++) {
        if (arr[i] < pivot) {
            smallArr.push(arr[i]);
        } else {
            largeArr.push(arr[i]);
        }
    }
    return quickSort(smallArr).concat(pivot, quickSort(largeArr));
}
```



以上算法的demo : http://www.boatsky.com/static/js/demo/sort_demo.js


这里写一个简单的程序，对上述算法时间计算，各个数组10000个数字，计算100次，取平均值，此处以快速排序为例


```javascript
//分别是每次的计算的开始时间与结束时间
var d1,d2;
//保存每次计算的时候
var arrTime = [];
//随机生成数组元素
//以为是计算时间例子
//分别是每次的计算的开始时间与结束时间
var d1,d2;
//保存每次计算的时候
var arrTime = [];
//随机生成数组元素
function getArr(){
    var arr = [];
    for(var i = 0;i < 10000;i++){
        arr.push(Math.floor(Math.random()*10000));
    }
    return arr;
}
//快速排序

//获取时间
function getTime(){
    //计算100次，取平均值，减小误差
    for(var k = 0;k < 100;k++){
        var arr = getArr();
        d1 = new Date().getTime();
        arr = quickSort(arr);
        d2 = new Date().getTime();
        arrTime.push(d2-d1);
    }
    var all = 0;
    for(var m = 0;m < arrTime.length;m++){
        all = all + arrTime[m];
    }
    console.log(arrTime);
    console.log(all/arrTime.length);
}
getTime();
```



统计100次  平均时间(ms)	时间复杂度	            空间复杂度    	   稳定性
冒泡排序	   704.69	   O(n(n+1)/2)	           O(1)	               稳定
选择排序	   214.31	   O(n(n+1)/2)	           O(1)	               稳定
插入排序	   50.62	   O(n)~O(n^2/2)           O(1)	               不稳定
归并排序	   2.16	       O(n log n)	           O(n)	               稳定
希尔排序	   1.5	       O(n log n) ~ O(n^2)     O(1)	               不稳定
快速排序	   0.15	       O(n log 2 n) ~ O(n^2)   O(log 2 n) ~ O(n)	不稳定


所以说，上述6种常见算法中，快速排序是最快的，但如果数据量太大时，内存占用大，而希尔排序速度较快，占内存小，稳定性略差。多数情况下都推荐这两种。

所以，根据你数据的类型，数据量的大小，机器内存的大小，进行一定的测试，选择最适合你的排序吧。

所以说，不要在任何情况下都用冒泡排序啦！！！
