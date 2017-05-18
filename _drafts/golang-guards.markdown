golang 中的 `defer` 机制可以用来函数返回时的清理。如果一个函数返回 `func()` 类型的结果就可以称为 guard。



{% highlight golang %}
func guard() func() {

​	fmt.Println("guards…")

​	return func() {

​		fmt.Println("leaves...")

​	}

}



func f() {

​	defer guard()()



​	fmt.Println("do something...")

}

{% endhighlight %}