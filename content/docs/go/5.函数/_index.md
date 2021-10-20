---
title: 5.函数
bookCollapseSection: true
weight: 5
---

# 5.函数
## 函数是一等公民
* 可以有多个返回值
* 所有参数都是值传递: slice, map, channel 会有传引用的错觉
* 函数可以作为变量的值
* 函数可以作为参数和返回值

## 可变参数
```aidl
func Sum(ops ...int) int {
	ret := 0;
	for _, op:= range ops {
		ret += op
	}
	return ret
}
```

## defer延迟运行
```aidl

```



