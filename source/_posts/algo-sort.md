---
title: 排序算法
date: 2021-07-03 18:03:40
mathjax: true
tags:
  - 算法
  - 排序
---

# 排序算法特点总结

| 排序算法     | 是否稳定 | 是否原地排序 | 时间复杂度                   | 空间复杂度 |
| ------------ | :------: | :----------: | ---------------------------- | ---------- |
| 选择排序     |    否    |      是      | _N_ <sup>2</sup>             |            |
| 插入排序     |    是    |      是      | _N_ 到 _N_ <sup>2</sup> 之间 |            |
| 希尔排序     |    否    |      是      |                              |            |
| 快速排序     |    否    |      是      | _N_ log*N*                   |            |
| 三向快速排序 |    否    |      是      |                              |            |
| 归并排序     |    是    |      否      | _N_ log*N*                   |            |
| 堆排序       |    否    |      是      | _N_ log*N*                   |            |

# 典型实现

### 选择排序

```go
func Sort(array []int) {
  for i := 0; i < len(array); i++ {
    minIndex := i
    for j := i + 1; j < len(array); j++ {
      if array[minIndex] > array[j] {
        minIndex = j
      }
    }

    array[minIndex], array[i] = array[i], array[minIndex]
  }
}
```

### 插入排序

```go
func Sort(array []int) {
  for i := 1; i < len(array); i++ {
    for j := i; j > 0 && array[j] < array[j-1]; j-- {
      array[j], array[j-1] = array[j-1], array[j]
    }
  }
}
```

### 希尔排序

```go
func Sort(array []int) {
  h := 1
  for h < len(array)/3 {
    h = 3*h + 1
  }

  for h >= 1 {
    for i := h; i < len(array); i++ {
      for j := i; j >= h && array[j] < array[j-h]; j -= h {
        array[j], array[j-h] = array[j-h], array[j]
      }
    }

    h = h / 3
  }
}
```

### 快速排序

```go
func Sort(array []int) {
  if len(array) <= 1 {
    return
  }

  mIndex := partition(array)

  Sort(array[0:mIndex])
  Sort(array[mIndex+1:])
}

func partition(array []int) int {
  lIndex, rIndex := 1, len(array)-1

  for {
    for array[lIndex] <= array[0] && lIndex < len(array)-1 {
      lIndex += 1
    }

    for array[0] < array[rIndex] && rIndex > 0 {
      rIndex -= 1
    }

    if lIndex >= rIndex {
      break
    }

    array[lIndex], array[rIndex] = array[rIndex], array[lIndex]
  }

  array[0], array[rIndex] = array[rIndex], array[0]

  return rIndex
}
```

### 归并排序

```go
func Sort(array []int) {
  if len(array) <= 1 {
    return
  }

  mid := len(array) / 2

  Sort(array[0:mid])
  Sort(array[mid:])

  merge(array, mid)
}

func merge(array []int, mid int) {
  tmp := make([]int, len(array))
  copy(tmp, array)

  i, j := 0, mid

  for k := 0; k < len(array); k++ {
    switch {
    case i >= mid:
      array[k] = tmp[j]
      j++
    case j >= len(array):
      array[k] = tmp[i]
      i++
    case tmp[i] < tmp[j]:
      array[k] = tmp[i]
      i++
    default:
      array[k] = tmp[j]
      j++
    }
  }
}
```

### 堆排序

```go
func Sort(array []int) {
  for i := len(array)/2 - 1; i >= 0; i-- {
    sink(array, i)
  }

  for i := len(array) - 1; i > 0; i-- {
    array[0], array[i] = array[i], array[0]

    sink(array[0:i-1], 0)
  }
}

func sink(array []int, index int) {
  for 2*index+1 < len(array) {
    cIndex := 2*index + 1
    if cIndex+1 < len(array) && array[cIndex] < array[cIndex+1] {
      cIndex++
    }

    if array[cIndex] <= array[index] {
      break
    }

    array[cIndex], array[index] = array[index], array[cIndex]

    index = cIndex
  }
}
```
