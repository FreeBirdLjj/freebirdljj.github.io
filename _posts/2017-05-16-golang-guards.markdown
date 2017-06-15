---
layout: post
title:  "Golang guards"
date:   2017-05-16 16:00:00 +0000
categories: jekyll update
---
golang 的 `defer` 机制用来在函数返回时执行清理操作。本文讲述的小技巧是利用返回类型为 `func()` 的函数作为 **guard function**。

```golang
func guard() func() {
	fmt.Println("Guarding...")
	return func() { fmt.Println("Leaving...") }
}

func f() {
	defer guard()()

	fmt.Println("Do something...")
}
```

`f()` 运行的输出是

```
Guarding...
Do something...
Leaving...
```

这样的函数作 **guard** 尤为方便，比如用来实现类似 C++ 的 `std::lock_guard`：

```golang
func LockGuard(m *sync.Mutex) func() {
	m.Lock()
	return m.Unlock
}
```

 之所以说是“类似”，是因为 C++ 中析构函数在离开当前作用域时被调用，而 golang 的 `defer` 则是在离开函数的时候被调用，生命周期有一点区别。

