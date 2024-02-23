### 参数传递，值传递，引用传递？

是否可以修改原内容数据，和传值，传引用没有必然的关系

**值传递：**是指在调用函数时将实际参数复制一份传递到函数中，在函数中如果对值进行修改，不会影响到实际参数

***Go没有引用传递***

当参数中是变量的指针时，实际上也是值传递，复制了一个指向相同内存空间的指针

```go
func createInt() *int {
    num := 42
    return &num   // Avoid returning a pointer to a local variable like this!
}

func createIntCorrect() *int {
    num := 42
    return new(int)  // Use new to allocate memory for the int
}

```

#### 在 Go 中，一个 goroutine 中的 panic 不会直接导致其他 goroutine 崩溃。每个 goroutine 都是独立的执行单元，它们之间是隔离的

但是在主 goroutine 中没有处理 panic 的情况下，整个程序可能会终止。

* 每次defer语句执行的时候，go会把它携带的defer函数及其参数值存储到一个链表中，这个链表叫做goroutine_defer。这个链表与defer语句所属的函数是对应的，先进后出，相当于一个栈

#### new%make

var关键字用来声明变量，而new和make函数用来分配内存

make用来分配及初始化类型为slice、map、chan的数据，返回的是类型本身

new可以分配任意类型的数据，并且置零

#### 逃逸分析

- 编译期无法确定的参数类型**必定**放到堆中；
- 如果变量在函数外部存在引用，则**必定**放在堆中；
- 如果变量占用内存较大时，则**优先**放到堆中；
- 如果变量在函数外部没有引用，则**优先**放到栈中；

#### 线程、协程

goroutine没有id,不能在一个协程中杀死另外一个协程，编码时需要考虑到协程什么时候创建，什么时候释放

协程存在于用户态，由go语言运行时调度器进行调度。协程从属于某一个线程，多个协程可以调度到一个线程中，一个协程也可能切换到多个线程中执行，因此协程与线程是多对多(M:N)的关系。

   