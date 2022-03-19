---
title: 四、反射和不安全编程
bookCollapseSection: true
weight: 4
---

# 四、反射和不安全编程
## 反射
### reflect.TypeOf vs reflect.ValueOf
* reflect.TypeOf 返回类型(reflect.Type)
* reflect.ValueOf 返回值(reflect.Value)
* 可以从reflect.Value获取类型
* 通过kind来判断类型

### 判断类型-Kind()
```$xslt
const (
	Invalid constant.Kind = iota
	Bool
	Int
	Int8
	Int16
	Int32
	Int64
	Uint
	Uint8
	Uint16
	Uint32
	Uint64
	...
)
```

### demo
```$xslt
func CheckType(v interface{})  {
	t := reflect.TypeOf(v)
	switch t.Kind() {
	case reflect.Float32, reflect.Float64 :
		fmt.Println("Float")
	case reflect.Int, reflect.Int8, reflect.Int16, reflect.Int32:
		fmt.Println("Integer")
	default:
		fmt.Println("Unknown", t)
	}
}

func TestCheckType(t *testing.T) {
	var f float64 = 12
	CheckType(f)
}
```

### 利用反射编写灵活的代码
#### 按名字访问结构的成员  
	reflect.ValueOf(*e).FieldByName("Name")
#### 按名字访问结构的方法  
	reflect.ValueOf(e).MethodByName("UpdateAge").Call([]reflect.Value {reflect.ValueOf(1)})

```$xslt
type Employee struct {
 	EmployeeID string
 	Name string `format:"normal"`
 	Age int
 }

 func (e *Employee) UpdateAge(newVal int) {
 	e.Age = newVal
 }

 type Customer struct {
 	CookieID string
 	Name string
 	Age int
 }

func TestInvokeByName(t *testing.T)  {
	e := &Employee{"1", "Mike", 30}
	// 按名称获取成员
	t.Logf("Name: value(%[1]v), Type(%[1]T) ", reflect.ValueOf(*e).FieldByName("Name"))
	if nameField, ok := reflect.TypeOf(*e).FieldByName("Name"); !ok {
		t.Error("Failed to get 'Name' field.")
	} else {
		t.Log("Tag:format", nameField.Tag.Get("format"))
	}
	reflect.ValueOf(e).MethodByName("UpdateAge").Call([]reflect.Value {reflect.ValueOf(1)})
	t.Log("Updated Age:", e)
}
```

#### struct Tag
```$xslt
type BasicInfo struct {
	Name string `json:"name"`
	Age int `json:"age"`
}

`json:"name"` 和 `json:"age"` 为struct Tag
```

#### 访问StructTag
```$xslt
	if nameField, ok := reflect.TypeOf(*e).FieldByName("Name"); !ok {
		t.Error("Failed to get 'Name' field.")
	} else {
		t.Log("Tag:format", nameField.Tag.Get("format"))
	}
Reflect.Type 和 Reflect.Value 都有  FieldByName 方法, 注意他们的区别
```

## 万能程序
### DeepEqual 
*比较切片和map*
```$xslt
func TestDeepEqual(t *testing.T) {
	a := map[int]string{1:"one", 2:"two", 3:"three"}
	b := map[int]string{1:"one", 2:"two", 3:"three"}
	//t.Log(a == b) // invalid operation: a == b (map can only be compared to nil)
	t.Log(reflect.DeepEqual(a, b))

	s1 := []int{1, 2, 3}
	s2 := []int{1, 2, 3}
	s3 := []int{2, 3, 1}
	t.Log("s1 == s2", reflect.DeepEqual(s1, s2))
	t.Log("s1 == s3", reflect.DeepEqual(s1, s3))
}
```

### 关于"反射"你应该知道的
* 提高了程序的灵活性
* 降低了程序的可读性
* 降低了程序的性能

```$xslt

type Employee struct {
	EmployeeID string
	Name string `format:"normal"`
	Age int
}

func (e *Employee) UpdateAge(newVal int) {
	e.Age = newVal
}

type Customer struct {
	CookieID string
	Name string
	Age int
}

func fillBySettings(st interface{}, settings map[string]interface{}) error {
	if reflect.TypeOf(st).Kind() != reflect.Ptr {
		if reflect.TypeOf(st).Elem().Kind() != reflect.Struct {
		    return errors.New("the first param should be a pointer to the struct type")
		}
	}

	if settings == nil {
		return errors.New("settings is nil.")
	}

	var (
		field reflect.StructField
		ok bool
	)

	for k, v := range settings {
	    if field, ok = (reflect.ValueOf(st)).Elem().Type().FieldByName(k); !ok {
	    	continue
		}
		if field.Type == reflect.TypeOf(v) {
		    vstr := reflect.ValueOf(st)
		    vstr = vstr.Elem()
		    vstr.FieldByName(k).Set(reflect.ValueOf(v))
		}
	}
	return nil
}

func TestFillNameAndAge(t *testing.T) {
	settings := map[string]interface{}{"Name":"Mike", "Age":10}
	e := Employee{}
	if err := fillBySettings(&e, settings); err != nil {
		t.Fatal(err)
	}
	t.Log(e)
	c := new(Customer)
	if err := fillBySettings(c, settings); err != nil {
		t.Fatal(err)
	}
	t.Log(*c)
}
```

## 不安全编程
### "不安全"行为的危险性
```$xslt
i := 10
f := *(*float64)(unsafe.Point(&i))
```

```$xslt
func TestAtomic(t *testing.T) {
	var shareBufferPtr unsafe.Pointer
	writeDataFn := func() {
		data := []int{}
		for i := 0; i < 100; i++ {
			data = append(data, i)
        }
        atomic.StorePointer(&shareBufferPtr, unsafe.Pointer(&data))
	}
	readDataFn := func() {
	    data := atomic.LoadPointer(&shareBufferPtr)
	    fmt.Println(data, *(*[]int)(data))
	}
	var wg sync.WaitGroup
	writeDataFn()
	for i := 0; i < 100; i++ {
		wg.Add(i)
		go func() {
			for i := 0; i < 100; i++ {
				writeDataFn()
				time.Sleep(time.Millisecond * 100)
            }
            wg.Done()
		}()
		wg.Add(1)
		go func() {
			for i := 0; i < 100; i++{
			    readDataFn()
			    time.Sleep(time.Millisecond * 100)
			}
			wg.Done()
		}()
	}
}
```



