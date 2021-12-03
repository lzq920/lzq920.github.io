---
title: 常用排序算法算法
date: 2021-12-03 14:00
---

使用 GitHub Copilot 两分钟实现常用的排序算法

```js
//冒泡排序
function bubbleSort(arr) {
    var len = arr.length;
    for (var i = 0; i < len; i++) {
        for (var j = 0; j < len - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                var temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
            }
        }
    }
    return arr;
}
console.log(bubbleSort([3, 2, 1, 4, 5, 6, 7, 8, 9, 10]));
//选择排序
function selectionSort(arr) {
    var len = arr.length;
    var minIndex, temp;
    for (var i = 0; i < len - 1; i++) {
        minIndex = i;
        for (var j = i + 1; j < len; j++) {
            if (arr[j] < arr[minIndex]) {
                minIndex = j;
            }
        }
        temp = arr[i];
        arr[i] = arr[minIndex];
        arr[minIndex] = temp;
    }
    return arr;
}
console.log(selectionSort([3, 2, 1, 4, 5, 6, 7, 8, 9, 10]));
//插入排序
function insertionSort(arr) {
    var len = arr.length;
    var preIndex, current;
    for (var i = 1; i < len; i++) {
        preIndex = i - 1;
        current = arr[i];
        while (preIndex >= 0 && arr[preIndex] > current) {
            arr[preIndex + 1] = arr[preIndex];
            preIndex--;
        }
        arr[preIndex + 1] = current;
    }
    return arr;
}
console.log(insertionSort([3, 2, 1, 4, 5, 6, 7, 8, 9, 10]));
//希尔排序
function shellSort(arr) {
    var len = arr.length,
        temp,
        gap = 1;
    while (gap < len / 5) {
        gap = gap * 5 + 1;
    }
    for (gap; gap > 0; gap = Math.floor(gap / 5)) {
        for (var i = gap; i < len; i++) {
            temp = arr[i];
            for (var j = i - gap; j >= 0 && arr[j] > temp; j -= gap) {
                arr[j + gap] = arr[j];
            }
            arr[j + gap] = temp;
        }
    }
    return arr;
}
console.log(shellSort([3, 2, 1, 4, 5, 6, 7, 8, 9, 10]));
//归并排序
function mergeSort(arr) {
    if (arr.length <= 1) {
        return arr;
    }
    var middle = Math.floor(arr.length / 2),
        left = arr.slice(0, middle),
        right = arr.slice(middle);
    return merge(mergeSort(left), mergeSort(right));
}
function merge(left, right) {
    var result = [];
    while (left.length && right.length) {
        if (left[0] <= right[0]) {
            result.push(left.shift());
        } else {
            result.push(right.shift());
        }
    }
    while (left.length) {
        result.push(left.shift());
    }
    while (right.length) {
        result.push(right.shift());
    }
    return result;
}
console.log(mergeSort([3, 2, 1, 4, 5, 6, 7, 8, 9, 10]));
//快速排序
function quickSort(arr) {
    if (arr.length <= 1) {
        return arr;
    }
    var pivotIndex = Math.floor(arr.length / 2),
        pivot = arr.splice(pivotIndex, 1)[0],
        left = [],
        right = [];
    for (var i = 0; i < arr.length; i++) {
        if (arr[i] < pivot) {
            left.push(arr[i]);
        } else {
            right.push(arr[i]);
        }
    }
    return quickSort(left).concat([pivot], quickSort(right));
}
console.log(quickSort([3, 2, 1, 4, 5, 6, 7, 8, 9, 10]));
//堆排序
function heapSort(arr) {
    var len = arr.length;
    buildMaxHeap(arr);
    for (var i = len - 1; i > 0; i--) {
        swap(arr, 0, i);
        len--;
        heapify(arr, 0, len);
    }
    return arr;
}
function buildMaxHeap(arr) {
    var len = arr.length;
    for (var i = Math.floor(len / 2); i >= 0; i--) {
        heapify(arr, i, len);
    }
}
function heapify(arr, i, len) {
    var left = 2 * i + 1,
        right = 2 * i + 2,
        largest = i;
    if (left < len && arr[left] > arr[largest]) {
        largest = left;
    }
    if (right < len && arr[right] > arr[largest]) {
        largest = right;
    }
    if (largest != i) {
        swap(arr, i, largest);
        heapify(arr, largest, len);
    }
}
function swap(arr, i, j) {
    var temp = arr[i];
    arr[i] = arr[j];
    arr[j] = temp;
}
console.log(heapSort([3, 2, 1, 4, 5, 6, 7, 8, 9, 10]));
//计数排序
function countingSort(arr) {
    var len = arr.length,
        bucketLen = Math.max.apply(null, arr) + 1,
        bucket = new Array(bucketLen),
        result = new Array(len);
    for (var i = 0; i < bucketLen; i++) {
        bucket[i] = 0;
    }
    for (var j = 0; j < len; j++) {
        bucket[arr[j]]++;
    }
    for (var k = 0, x = 0; k < bucketLen; k++) {
        while (bucket[k]-- > 0) {
            result[x++] = k;
        }
    }
    return result;
}
console.log(countingSort([3, 2, 1, 4, 5, 6, 7, 8, 9, 10]));

```
