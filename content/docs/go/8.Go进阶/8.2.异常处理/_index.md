---
bookCollapseSection: true
weight: 82
---

# 8.2.异常处理
## ERROR
go error就是普通的一个接口,普通的值
```$xslt
# http://golang.org/pkg/builtin/#error
type error interface {
    Error() string
}
```
我们经常使用errors.New()来返回一个error对象
```$xslt
# https://golang.org/src/errors/errors.go
type errorString struct {
	s string
}

func (e *errorString) Error() string {
	return e.s
}
```

基础库中大量自定义的error
{{< img src="/architect/go/8.2/1.png" title="" >}}

errors.New() 返回的是内部 errorString 对象的指针。
{{< img src="/architect/go/8.2/2.png" title="" >}}

Output: Named Type Error
{{< img src="/architect/go/8.2/3.png" title="" >}}

Output: Error: EOF
{{< img src="/architect/go/8.2/4.png" title="" >}}

## Error vs Exception
Go 的处理异常逻辑是不引入 exception，支持多参数返回，所以你很容易的在函数签名中带上实现了 error interface 的对象，交由调用者来判定。

如果一个函数返回了 value, error，你不能对这个 value 做任何假设，必须先判定 error。唯一可以忽略 error 的是，如果你连 value 也不关心。

Go 中有 panic 的机制，如果你认为和其他语言的 exception 一样，那你就错了。当我们抛出异常的时候，相当于你把 exception 扔给了调用者来处理。

比如，你在 C++ 中，把 string 转为 int，如果转换失败，会抛出异常。或者在 java 中转换 string 为 date 失败时，会抛出异常。
Go panic 意味着 fatal error(就是挂了)。不能假设调用者来解决 panic，意味着代码不能继续运行。

使用多个返回值和一个简单的约定，Go 解决了让程序员知道什么时候出了问题，并为真正的异常情况保留了 panic。

{{< img src="/architect/go/8.2/5.png" title="" >}}
{{< img src="/architect/go/8.2/6.png" title="" >}}
{{< img src="/architect/go/8.2/7.png" title="" >}}
{{< img src="/architect/go/8.2/8.png" title="" >}}
{{< img src="/architect/go/8.2/9.png" title="" >}}
{{< img src="/architect/go/8.2/10.png" title="" >}}

对于真正意外的情况，那些表示不可恢复的程序错误，例如索引越界、不可恢复的环境问题、栈溢出，我们才使用 panic。对于其他的错误情况，我们应该是期望使用 error 来进行判定。

you only need to check the error value if you care about the result.  -- Dave  

This blog post from Microsoft’s engineering blog in 2005 still holds true today, namely:

My point isn’t that exceptions are bad. My point is that exceptions are too hard and I’m not smart enough to handle them.

* 简单。
* 考虑失败，而不是成功(Plan for failure, not success)。
* 没有隐藏的控制流。
* 完全交给你来控制 error。
* Error are values。
```
item = getFromDB()
item.Value = 400
saveToDB(item)
item.Text = 'price changed'
```

## Error Type
### Sentinel Error
预定义的特定错误，我们叫为 sentinel error，这个名字来源于计算机编程中使用一个特定值来表示不可能进行进一步处理的做法。所以对于 Go，我们使用特定的值来表示错误。

if err == ErrSomething { … }  
类似的 io.EOF，更底层的 syscall.ENOENT。  

使用 sentinel 值是最不灵活的错误处理策略，因为调用方必须使用 == 将结果与预先声明的值进行比较。当您想要提供更多的上下文时，这就出现了一个问题，因为返回一个不同的错误将破坏相等性检查。

甚至是一些有意义的 fmt.Errorf 携带一些上下文，也会破坏调用者的 == ，调用者将被迫查看 error.Error() 方法的输出，以查看它是否与特定的字符串匹配。

* 不依赖检查 error.Error 的输出。
不应该依赖检测 error.Error 的输出，Error 方法存在于 error 接口主要用于方便程序员使用，但不是程序(编写测试可能会依赖这个返回)。这个输出的字符串用于记录日志、输出到 stdout 等。

## Handling Error
## Go 1.13 errors
## Go 2 Error Inspection
