---
title: 一、基础
bookCollapseSection: true
weight: 1
---

# 一.基础
## 1、第一个go程序
```$xslt
package main    //包,表明代码所在的模块(包)
  
import "fmt" // 引入代码依赖

// 功能实现
func main() {
    fmt.Println("hello world")
}
```
### 1.1、应用程序入口
* 必须是main包: package main
* 必须是main方法: func main()
* 文件名不一定是main.go

### 1.2、退出返回值
*与其他主要编程语言的差异*
* Go中main函数不支持任何返回值
* 通过os.Exit来返回状态

### 1.3、获取命令行参数
*与其他主要编程语言的差异*
* main函数不支持传入参数
    func main(~~arg []string~~)
* 在程序中直接通过os.Args获取命令行参数

### 1.3、编写测试程序
* 源码文件以 _test结尾: xxx_test.go
* 测试方法名以Test方法开头: func TestXXX(t *testing.T) {...}

## 2、基本程序结构
### 2.1、变量赋值
*与其他主要编程语言的差异*
* 赋值可以进行自动类型推断
* 在一个赋值语句中可以对多个变量进行同时赋值

### 2.2、常量定义
*与其他主要编程语言的差异*

**快速设置连续值**
```$xslt
const (
	Monday = iota + 1
	Tuesday
	Webnesday
	Thursday
	Friday
	Staturday
	Sunday
)
```

### 2.3、数据类型
```$xslt
bool
string
int int8 int16 int32 int64
uint uint8 uint16 uint32 uint64 uintptr
byte // alias for uint8
rune // alias for int32 represents a unicode code point
float32 float64
complex64 complex128
```

#### 2.3.1、类型转换
*与其他主要编程语言的差异*
* Go语言不允许隐式类型转换
* 别名和原有类型也不能进行隐式类型转换

#### 2.3.2、类型的预定义值
* math.MaxInt64
* math.MaxFloat64
* math.MaxUint32

#### 2.3.3、指针类型
*与其他主要编程语言的差异*
* 不支持指针运算
* string是值类型,其默认的初始化值为空字符串,而不是nil

### 2.4、运算符
#### 2.4.1、算术运算符
没有前置的++ --

#### 2.4.2、比较运算符
*与其他主要编程语言的差异*
*用==比较数组*
* 相同维数且含有相同个数元素的数组才可以比较
* 每个元素都相同的才相等
#### 2.4.3、逻辑运算符
和主流语言相同
#### 2.4.4、位运算符
*与其他主要编程语言的差异*
&^ 按位置零
```$xslt
1 &^ 0  -- 1
1 &^ 1  -- 0
0 &^ 1  -- 0
0 &^ 0  -- 0
右边的二进制是0，左边的是什么，结果就是什么
右边的二进制是1，左边的无论是什么, 结果都是0
```

### 2.5、循环
*与其他主要编程语言的差异*
* Go语言仅支持循环关键字for
* for ~~(~~ j := 7; j <= 9; j++  ~~)~~ //不需要"("和")"。
```$xslt
while条件循环 while(n<5)
n := 0
for n < 5 {
    n++
    fmt.Println(n)
}

无限循环 while(true)
n := 0
for {
...
}
```

### 2.6、if条件
```$xslt
if condition {
    // code to be executed if condition is true
} else {
    // code to be executed if condition is false
}

if condition-1 {
    // code to be executed if condition-1 is true
} else if condition-2 {
    // code to be executed if condition-2 is true
} else {
    // code to be executed if both condition1 and condition2 are false
}
```
*与其他主要编程语言的差异*
* condition  表达式结果必须为布尔值
* 支持变量赋值:
```$xslt
if var declaration; condition {
    // code to be executed if condition is true
}
```
### 2.7、switch条件
```$xslt
switch os := runtime.GOOS; os {
    case "darwin":
        fmt.Println("OS x.")
    case "linux":
        fmt.Println("Linux.")
    default:
        fmt.Printf("%s.", os)
}

switch {
    case 0 <= Num && Num <= 3:
        fmt.Printf("0-3")
    case 4 <= Num && Num <= 6:
        fmt.Printf("4-6")
    case 7 <= Num && Num <= 9:
        fmt.Printf("7-9")
}
```
*与其他主要编程语言的差异*
* 条件表达式不限制为常量或者整数
* 单个case中，可以出现多个结果选项，使用逗号分隔；
* 与C语言等规则相反,GO语言不需要用break来明确退出一个case
* 可以不设定switch之后的条件表达式，在此种情况下,整个switch结构与多个if...else...的逻辑作用等同

## 3、常用集合
### 3.1、数组
#### 3.1.1、数组的声明
```$xslt
var a [3]int  // 声明并初始化为默认零值
a[0] = 1

b := [3]int{1,2,3}   // 声明同时初始化
c := [...]int{1,2,3}   // 声明同时初始化
d := [2][2]int{{1,2}, {3, 4}} // 多维数组初始化
```
#### 3.1.2、数组截取
a[开发索引(包含), 结果索引(不包含)]  
注:开发索引 和 结果索引 都不支持负数

### 3.2、切片
#### 3.2.1、切片内部结构
ptr: 指针   
len: 元素的个数   
cap: 内部数组的容量

#### 3.2.2、切片声明
```$xslt
var s0 []int
s0 = append(s0, 1)

s := []int{]
s1 := []int{1, 2, 3}
s2 := make([]int, 2, 4)
/* []type, len, cap
其中len个元素会被初始化为默认零值,未初始化元素不可以访问
*/
```
#### 3.2.2、切片共享存储结构
*切片是如何实现可变长的?*

### 3.3、数组vs切片
* 容量是否可伸缩
    数据不可伸缩, 切片可伸缩
* 是否可以进行比较
    数组相同大小，相同类型能比较，切片不能比较
    
    
### 3.3、Map
#### 3.3.1、Map声明
````$xslt
m := map[string]int{"one":1, "two": 2}

m1 := map[string]int{}
m1["three"] = 3

m2 := make(map[string]int, 10 /*Initial Capacity*/)
// 为什么不初始化len

````

#### 3.3.2、Map元素的访问
*与其他主要编程语言的差异*
在访问的key不存在时,仍会返回零值, 不能通过返回nil来判断元素是否存在
```$xslt
if v, ok := m1[3]; ok {
		t.Logf("key 3's value is %d", v)
	} else {
		t.Log("key 3 is not existing.")
	}
```
#### 3.3.3、Map遍历
```$xslt
m := map[string]int{"one":1, "two":2, "three": 3}
for k, v := range m {
    t.Log(k, v)
}
```

#### 3.3.4、Map与工厂模式
* map的value可以是一个方法
* 与go的Dock type接口方式一起, 可以方便的实现单一方法对象的工厂模式

#### 3.3.5、线程安全map 
https://github.com/easierway/concurrent_map


#### 3.3.6、实现Set
Go的内置集合中没有Set实现,可以map[type]bool
* 元素的唯一性
* 基本操作
    * 添加元素
    * 判断元素是否存在
    * 删除元素
    * 元素个数
 ```$xslt
    // 定义初始化
    mySet := map[int]bool{}
	// s和值1
	mySet[1] = true
	
	// 判断是否3存在
	n := 3
	if mySet[n] {
		t.Logf("%d is existing", n)
	} else {
		t.Logf("%d is not existing", n)
	}
	// 添加key 3
	mySet[3] = true 
	t.Log(len(mySet))

	// 删出key 1
	delete(mySet, 1)

	// 判断key 1 是否存在
	if mySet[1] {
		t.Logf("%d is existing", 1)
	} else {
		t.Logf("%d is not existing", 1)
	}
```

## 4、字符串
*与其他主要编程语言的差异*
* string是数据类型，不是引用或指针类型
* string是只读的byte slice，len函数可以返回它所包含的byte数
* string的byte数组可以存放任何数据

### 4.1、Unicode UTF8
* Unicode是一种字符集 (code point)
* UTF8 是unicode的存储实现(转换为字节序列的规则)

### 4.2、编码与存储
|字符|"中"|
|---|---|
|Unicode|0x4E2D|
|UTF-8|0xE4B8AD|
|string/[]byte|[0xE4,0xB8,0xAD]|

### 4.3、常用字符串函数
* strings包 (https://golang.org)
* strconv 包 (https://golang.org/pkg/strconv)

## 5、函数
### 5.1、函数是一等公民
*与其他主要编程语言的差异*
* 可以有多个返回值
* 所有参数都是值传递: slice, map, channel会有传引用的错觉
* 函数可以作为变量的值
* 函数可以作为参数和返回值

### 5.2、可变参数及defer
#### 5.2.1、可变参数
```$xslt
func sum(ops ...int) int {
	s := 0
	for _, op := range ops {
	    s += op
	}
	return s
}
```
#### 5.2.2、defer
```$xslt
func TestDefer(t *testing.T) {
	defer func() {
		t.Log("clear resources")
	}()
	t.Log("Started")
	panic("Fatal error") // defer仍会执行
}
```
## 6、面向对象编程
### 6.1、结构体定义
```$xslt
type Employee struct {
	Id string
	Name string
	Age int
}
e := Employee {Id : "1"}
e1 := new(Employee) // 注意这里返回的引用/指针,相当于e:=&Employee{}
e1.Id = "2"   // 与其他主要编程语言的差异；通过实例的指针访问成员不需要使用->
```
### 6.2、行为(方法)定义
```$xslt
// 第一种定义方式在实例对应方法被调用时，实例的成员会进行值复制
func (e Employee) String() string {
    return fmt.Sprintf("ID:%s-Name:%s-Age:%d", e.Id, e.Name, e.Age)
}

// 通常情况下为了避免内存拷贝我们使用第二种定义方式
func (e *Employee) String() string {
	return fmt.Sprintf("ID:%s-Name:%s-Age:%d", e.Id, e.Name, e.Age)
}
```
### 6.3、Duck Type 式接口
```$xslt
type Programmer interface {
	WriteHelloWorld() string
}

type GoProgrammer struct {
}

func (g *GoProgrammer) WriteHelloWorld() string {
	return "fmt.Println(\"Hello World\")"
}
```
*与其他主要编程语言的差异*
* 接口为非入侵性,实现不依赖于接口定义
* 所有接口的定义可以包含在接口使用者包内

### 6.4、接口变量
```$xslt
var prog  Code = &GoProgrammer{} 

// 类型
type GoProgrammer struct {
}

// 数据
&GoProgrammer{}
```

### 6.5、自定义类型
* type IntConvertionFn func(n int) int
* type MyPoint int

### 6.6、扩展与复用
*与其他主要编程语言的差异*
不支持重载

### 6.7、多态
```$xslt
package duotai

import (
	"fmt"
	"testing"
)

type Programmer interface {
	WriteHelloWorld() string
}

type GoProgrammer struct {

}

func (p *GoProgrammer) WriteHelloWorld() string {
	return "fmt.Println(\"hello world!\")"
}

type JavaProgrammer struct {
}

func (p *JavaProgrammer) WriteHelloWorld() string {
	return "fmt.Println(\"hello world!\")"
}

func writeFirstProgram(p Programmer) {
	fmt.Printf("%T %v\n", p, p.WriteHelloWorld())
}

func TestPolymorphism(t *testing.T) {
	goProg := new(GoProgrammer)
	javaProg := new(JavaProgrammer)
	writeFirstProgram(goProg)
	writeFirstProgram(javaProg)
}
```
### 6.8、空接口与断言
* 空接口可以表示任何类型
* 通过断言来将空接口转换为指定类型
```$xslt
v, ok := p.(int) // ok == true 时为转换成功
```

### 6.9、Go接口的最佳实践
* 倾向于使用小的接口定义,很多接口只包含一个方法
```$xslt
type Reader interface{
    Read(p []byte) (n int, err error)
}

type Writer interface {
    Write(p []byte) (n int, err error)
}
```
* 较大的接口定义,可以由多个小接口定义组合而成
```$xslt
type ReadWriter interface{
    Reader
    Writer
}
```

* 只依赖于必要功能的最小接口
```$xslt
func Store(reader Reader) error {
	...
})
```

## 7、错误处理
### 7.1、Go的错误机制
*与其他主要编程语言的差异*
* 没有异常机制
* error类型实现了error接口
```$xslt
type error interface {
    Error() string
}
```
* 可以通过errors.New来快速创建错误实例
```$xslt
errors.New("n must be in ther r ange [0, 100]")
```
*最佳实践*  
定义不同的错误变量,1️以便于判断错误类型
```$xslt
var LessThanTwoError error = errors.New("n must be grater than 2")
var GreaterThanHundredError error = errors.New("n must be less than 100")

func TestGetFibonacci(t *testing.T) {
    var list []int
    list, err := GetFibonacci(-10)
    if err = LessThanTwoError { 
        t.Error("Need a large Number")
    }

    if err = GreaterThanHundredError { 
        t.Error("Need a large Number")
    }
}
```
*及早失败,避免嵌套* 

### 7.2、panic vs recover
* os.Exit退出时不会调用defer指定的函数
* os.Exit退出时不输出当前调用栈信息
```$xslt
defer func() {
    if err := recover(); err != nil {
        fmt.Println("recoverd from", err)
    }
    fmt.Println("Finally!")
}()
```
#### 7.2.1、最常见的"错误恢复"
```$xslt
defer func() {
    if err := recover(); err != nil {
        fmt.Println("recoverd from", err)
    }
    fmt.Println("Finally!")
}()
```
*当心！recover成为恶魔*
* 形成僵尸服务进程,导致health check失败。
* "Let it Crash!"往往是我们恢复不确定性错误的最好方法。

## 8、包和依赖管理
### 8.1、构建可复用的模块
* 基本复用模块单元  
    以首字母大写来表名可被包外代码访问
* 代码的package可以和所在的目录不一致
* 同一目录里的Go代码的package要保持一致
* 在main被执行前,所有依赖的package的init方法都会被执行
* 不同包的init函数按照包导入的依赖关系决定执行顺序
* 每个包可以有多个init函数
* 包的每个源文件也可以有多个init函数,这点比较 特殊

### 8.2、go mod 依赖管理














