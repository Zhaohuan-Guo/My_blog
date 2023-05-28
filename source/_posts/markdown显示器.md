---
title: markdown显示器
date: 2023-05-27 16:55:12
tags:
    - markdown
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1605362981730/wDYGLi6rK.png
---
以下是 JavaScript 实现最简洁的二分查找的代码：

```javascript
function binarySearch(arr, target) {
  let left = 0,
      right = arr.length - 1;
  while (left <= right) {
    let mid = Math.floor((left + right) / 2);
    if (arr[mid] === target) {
      return mid;
    } else if (arr[mid] < target) {
      left = mid + 1;
    } else {
      right = mid - 1;
    }
  }
  return -1;
}
```

这段代码函数`binarySearch()`接受两个参数：一个有序数组 `arr` 和一个目标值 `target`。它使用了 `left` 和 `right` 两个指针来标识数组的左右边界，通过移动这两个指针的位置不断缩小范围，最终找到目标值的位置。

具体实现中，我们使用了一个 `while` 循环来检查左右边界是否交叉。同时，我们用 `mid` 来标识中间位置的索引，如果查找的值等于 `arr[mid]`，那么直接返回该位置。如果目标值小于 `arr[mid]`，说明在左侧，将右边界缩小到`mid - 1`；否则目标值在右侧，将左边界增大到`mid + 1`。

最后，如果循环完仍未发现目标值，返回 -1。