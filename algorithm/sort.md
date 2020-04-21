## 排序

### 快速排序

```js
function quickSort(arr) {
    if (!Array.isArray(arr)) {
        throw new Error('not array');
    }
    arr = [...arr];
    if (arr.length < 2) {
        return arr;
    }
    let l = 0;
    let r = arr.length - 1;
    let b = arr[0];
    while(l < r) {
        while(arr[r] >= b && l < r) r--;
        arr[l] = arr[r];
        while(arr[l] <= b && l < r) l++;
        arr[r] = arr[l];
    }
    arr[l] = b;
    return [
        ...quickSort(arr.slice(0, l)),
        b,
        ...quickSort(arr.slice(l + 1))
    ];
}
```