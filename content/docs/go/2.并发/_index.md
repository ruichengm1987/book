---
title: 二、并发
bookCollapseSection: true
weight: 2
---

# 二.并发

## 多路复用
```$xslt
select {
case ret := <-retCh1:
    t.Logf("result %s", ret)
case ret := <-retCh2:
    t.Logf("result %s", ret)
default:
    t.Error("no one returned")
}
如果执行到select时没有收到channel的消息的时候，如果有default就会执行default
```
## 超时
```$xslt
select {
case ret := <-retCh1:
    t.Logf("result %s", ret)
case <-time.After(time.Second * 1) // 超时
    t.Error("time out")
}
```
## channel的关闭与广播
* 向关闭的channel发送数据, 会导致panic
* v, ok <- ch; ok 为bool值, true表示正常接收, false表示通道关闭
* 所有的channel接收者都会在channel关闭时,立刻从阻塞等待中返回且上述ok值为false。这个广播机制被利用,进行向多个
订阅者同时发送信号。例如:退出信号
* 接收已经关闭的通道，将获取该通道类型的默认值

## 任务的取消
```$xslt
package cancel_task

import (
	"fmt"
	"testing"
	"time"
)

func isCancelled(cancelChan chan struct{}) bool {
	select {
	case <-cancelChan:
		return true
	default:
	    return false
	}
}

func cancel_1(cancelChan chan struct {}) {
	cancelChan <- struct {} {

	}
}

func cancel_2(cancelChan chan struct {}) {
	close(cancelChan)
}

func TestCancel(t *testing.T) {
	cancelChan  := make(chan struct {}, 0)
	for i := 0; i < 5; i++ {
		go func(i int, cancelCh chan struct{}) {
			for {
				if isCancelled(cancelChan) {
					break
				}
				time.Sleep(time.Millisecond * 5)
			}
			fmt.Println(i, "Cancelled()")
		}(i, cancelChan)
	}
	//cancel_1(cancelChan) // 此方法不可取
	cancel_2(cancelChan) // 广播机制
	time.Sleep(time.Millisecond * 1)
}
```

## context与任务取消
* 根Context:通过context.Background() 创建
* 子 Context: context.WithCancel(parentContext) 创建
    * ctx, cancel := context.WithCancel(context.Background())
* 当前Context被取消时,基于他的子context都会被取消
* 接收取消通知 <-ctx.Done()
```$xslt
package cancel_task

import (
	"context"
	"fmt"
	"testing"
	"time"
)

func isCancelled(ctx context.Context) bool {
	select {
	case <-ctx.Done():
		return true
	default:
	    return false
	}
}

func TestCancel(t *testing.T) {
	ctx, cancel := context.WithCancel(context.Background())
	for i := 0; i < 5; i++ {
		go func(i int, ctx context.Context) {
			for {
				if isCancelled(ctx) {
					break
				}
				time.Sleep(time.Millisecond * 5)
			}
			fmt.Println(i, "Cancelled()")
		}(i, ctx)
	}
	cancel()
	time.Sleep(time.Millisecond * 1)
}
```

## 仅运行一次
sync.once
```$xslt
package once_test

import (
	"fmt"
	"sync"
)

type SingletonObj struct {
    Id string
}

var once sync.Once
var obj *SingletonObj

func GetSingletonObj() *SingletonObj {
	once.Do(func() {
		fmt.Println("Create singleton obj")
		obj = &SingletonObj{}
	})
	return obj
}
```
## 仅需任意任务完成
```$xslt
package one_ok

import (
	"fmt"
	"runtime"
	"testing"
	"time"
)

func runTask(id int) string {
	time.Sleep(10 * time.Millisecond)
	return fmt.Sprintf("The result is from %d", id)
}

func FirstResponse() string {
	numOfRunner := 10
	ch := make(chan string, numOfRunner) // 使用buffer channel 防止协程泄露
	for i := 0; i < numOfRunner; i++ {
		go func(i int) {
			ret := runTask(i)
			ch <- ret
		}(i)
	}
	return <-ch
}

func TestFirstResponse(t *testing.T) {
	t.Log("Befor:", runtime.NumGoroutine())
	t.Log(FirstResponse())
	time.Sleep(time.Second * 1)
	t.Log("After:", runtime.NumGoroutine())
}

```

## 所有任务都完成
```$xslt
package mul_ok

import (
	"fmt"
	"runtime"
	"testing"
	"time"
)

func runTask(id int) string {
	time.Sleep(10 * time.Millisecond)
	return fmt.Sprintf("The result is from %d", id)
}

func AllResponse() string {
	numOfRunner := 10
	ch := make(chan string, numOfRunner) // 使用buffer channel 防止协程泄露
	for i := 0; i < numOfRunner; i++ {
		go func(i int) {
			ret := runTask(i)
			ch <- ret
		}(i)
	}
	finalRet := ""
	for j:=0;j< numOfRunner;j++ {
		finalRet += <-ch + "\n"
	}
	return finalRet
}

func TestFirstResponse(t *testing.T) {
	t.Log("Befor:", runtime.NumGoroutine())
	t.Log(AllResponse())
	time.Sleep(time.Second * 1)
	t.Log("After:", runtime.NumGoroutine())
}
```

## 对象池
### 使用buffer channel实现对象池
```$xslt
package pool_test

import (
	"errors"
	"fmt"
	"testing"
	"time"
)

type ReusableObj struct {
	
}

type ObjPool struct {
    bufChan chan *ReusableObj
}

func NewObjPool(numOfObj int) *ObjPool {
	objPool := ObjPool {}
	objPool.bufChan = make(chan * ReusableObj, numOfObj)
	for i := 0; i < numOfObj; i++ {
		objPool.bufChan <- &ReusableObj{

		}
	}
	return &objPool
}

func (p *ObjPool) GetObj(timeout time.Duration) (*ReusableObj, error) {
	select {
	case ret := <- p.bufChan:
		return ret, nil
	case <-time.After(timeout):
		return nil, errors.New("time out")
	}
}

func (p *ObjPool) ReleaseObj(obj *ReusableObj) error {
	select {
	case p.bufChan <- obj:
		return nil
	default:
		return errors.New("overflow")
	}
}

func TestObjPool(t *testing.T) {
	pool := NewObjPool(10)
	for i := 0; i < 11; i++ {
		if v, err := pool.GetObj(time.Second + 1); err != nil {
			t.Error(err)
		} else {
			fmt.Printf("%T\n", v)
			if err := pool.ReleaseObj(v); err != nil {
				t.Error(err)
			}

		}
	}
}
```

## sync.Pool对象缓存
* 尝试从私有对象获取
* 私有对象不存在，尝试从当前Processor的共享池获取
* 如果当前Processor共享池也是空的，那么就尝试去其他Processor的共享池获取
* 如果所有子池都是空的，最后就用用户指定的New函数产生一个新的对象返回

私有对象: 协程安全  
共享池: 协程不安全

### sync.Pool对象的放回
* 如果私有对象不存在则保存为私有对象
* 如果私有对象存在，放入当前Processor子池的共享池中
```$xslt
pool := &sync.Pool{
    New: func() interface{} {
        return 0
    },
}
arry := pool.Get().(int)
...
pool.Put(10)
```

### sync.Pool对象的生命周期
* GC会清除sync.pool缓存的对象
* 对象的缓存有效期为下一次GC之前

###  sync.Pool总结
* 适用于通过复用,降低复杂对象的创建和GC代价
* 协程安全, 会有锁的开销
* 生命周期受GC影响,不适合于做 连接池等，需要自己管理生命周期的资源的池化








