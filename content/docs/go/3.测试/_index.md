---
title: 三、测试
bookCollapseSection: true
weight: 3
---

# 三、测试
## 单元测试
```$xslt
func square(i int) int {
	return i * i
}

func TestSquare(t *testing.T) {
    inputs := [...]int{1, 2, 3}
    expected := [...]int{1, 4, 10}
    for i := 0; i < len(inputs); i++{
        ret := square(inputs[i])
        if ret != expected[i] {
            t.Errorf("input is %d, the expected is %d, " +
                "the actual is %d", inputs[i], expected[i], ret)
        }
    }
}

```

### 内置单元测试框架
* Fail,Error:该测试失败,该测试继续, 其他测试继续执行
* FailNow,Fatal:该测试失败,该测试终止，其他测试继续执行
```$xslt
func TestErrorInCode(t *testing.T) {
	fmt.Println("Start")
	t.Error("Error")
	fmt.Println("End")
}
//结果:
//Start
//demo_test.go:26: Error
//End

func TestFailInCode(t *testing.T) {
	fmt.Println("Start")
	t.Fatal("Fatal")
	fmt.Println("End")
}
//结果:
//Start
//demo_test.go:35: Fatal
```

* 代码覆盖率   
go test -v -cover

* 断言  
go get -u github.com/stretchr/testify/assert

## Benchmark
```$xslt
func BenchmarkConcatStringByAdd(b *testing.B) {
	// 与性能测试无关的代码
	b.ResetTimer()
	for i := 0; i < b.N; i++ {
		// 测试代码
	}
	b.StopTimer()
	// 与性能测试无关的代码
}
```
go test -bench=. -benchmem

## BDD in Go
### 项目网站
https://github.com/smartystreets/goconvey

### 安装
go get -u github.com/smartystreets/goconvey

### 启动 WEB UI
$GOPATH/bin/goconvey

