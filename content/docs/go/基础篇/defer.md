---
title: defer
weight: 2
---

# defer 
## 什么是defer
* defer是Go语言的一种用于注册延迟调用的机制,使得函数或语句可以在当前函数执行完毕后执行.

## 为什么需要defer
* Go语言提供的语法糖,减少资源泄露的发生.

## 如何使用defer
* 在创建资源语句的附近,使得defer语句释放资源. 

## 例子
```$xslt
func f1() (r int) {
    t := 5
    // 1. 赋值指令
    r = t

    // 2. defer被插入到赋值与返回质检， 这个例子中返回值r没被修改过
    defer func() {
       t = t + 5 
    }()

    // 3. 空的return指令
    return t
}
返回值是5

func f2() (r int) {
    defer func(r int) {
        r = r + 5
    }(r)  // 此处r是copy了一份
    return 1
}
返回值是1

func f3() (r int) {
    defer func(r *int) {
        *r = *r + 5
    }(&r)  // 此处r是传址
    return 1
}
返回值是6

defer 是先入先出

func e1() {
    var err error
    defer fmt.Println(err)
    err = errors.New("defer1 error")
    return
}

func e2() {
    var err error
    def func() {
        fmt.Println(err)
    }()
    err = errors.New("defer2 error")
    return
}

func e3() {
    var err error
    def func(err error) {
        fmt.Println(err)
    }(err)
    err = errors.New("defer3 error")
    return
}
main执行
e1()
e2()
e3()
结果是:
nil
defer2 error
nil

```

