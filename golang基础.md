#### channel -> 由hchan实现

* 元素个数、长度、环形队列指针、
* 等待读和等待写的协程队列(体现了GMP中阻塞的G被放在应用层解决)

panic操作：关闭值为nil的管道、关闭已经被关闭的管道、向已经关闭的管道写数据

使用互斥锁保证线程安全，

#### map -> 由hmap实现，渐进式扩容

$负载因子=键数量/bucket数量$ 其中当时负载因子大于6.5时进行扩容

选桶原则：$hash \& (m-1) $ (m为2的幂，保证不会出现空桶)

![go-map](D:\Work\1513-Leetcode\pic\go-map.png)

扩容过程：让oldbuckets成员指向原buckets数组，然后申请新的buckets数组（长度为原来的两倍），完成新老buckets数组的交接，随后逐步将oldbuckets数组中所有键值对搬迁完毕后释放oldbuckets数组。



#### WaitGroup (Add, Done, Wait)

* Add: 先把delta值累加到counter中，判断当counter<0触发panic，当counter==0，释放所有信号量

* Done: 等价于Add(-1)

* Wait: 先通过CAS算法累加信号量，成功后等待信号量唤醒

#### 自旋过程 Mutex

如果加锁的协程很多，每次都通过自旋获得锁，那么之前被阻塞的进程将很难获得锁，从而进入“饥饿”状态。

在“饥饿”模式下，不会启动自旋过程，即一旦有协程释放了锁，那么一定会唤醒协程。

#### GMP

**线程：**调度切换都要陷入内核，并且线程的栈空间不变，远大于goroutine

**goroutine:** G的数量要远大于M的数量，而且当G阻塞时，go会把G调度走，让其他G继续执行，而不是让线程阻塞 G的栈空间很小，只有几KB，而且可以动态扩缩容

**Processor:** 是一个抽象的概念，不是真正的物理CPU。可以理解为存储G的队列(本地队列)。

LocalQueue: 使用LockFree和CAS的方法实现轻锁化。

**GOMAXPROCS只能限制P的数量**，如果syscall太多，闲置的P可能会创建很多M，导致线程太多

*Work-stealing: 当P执行完本地所有G时，如果全局队列为空，则窃取别的P中的一半G，如果不为空，则从全局队列中窃取。*

防止全局队列中的饥饿现象，每N轮调度之后会从全局调度中拿一个G。



Syscall(G阻塞): 在当前的G调用syscall时，为了数据的局部性，P是不能被调度给别的M的，防止短时间内阻塞的M被唤醒。

但是当M阻塞时间过长，sys monitor会强行将P解绑



#### 参数传递，值传递，引用传递？

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

#### 在 Go 中，一个 goroutine 中的 panic 不会直接导致其他 goroutine 崩溃。

每个 goroutine 都是独立的执行单元，它们之间是隔离的.但是在主 goroutine 中没有处理 panic 的情况下，整个程序可能会终止。

* 每次defer语句执行的时候，go会把它携带的defer函数及其参数值存储到一个链表中，这个链表叫做goroutine_defer。这个链表与defer语句所属的函数是对应的，先进后出，相当于一个栈

#### new%make

var关键字用来声明变量，而new和make函数用来分配内存

make用来分配及初始化类型为slice、map、chan的数据，返回的是类型本身

new可以分配任意类型的数据，并且置零

#### 为什么不允许修改字符串

不像C++，string不含内存空间，只有一个内存的指针，十分轻量。

#### 逃逸分析

- 编译期无法确定的参数类型**必定**放到堆中；
- 如果变量在函数外部存在引用，则**必定**放在堆中；
- 如果变量占用内存较大时，则**优先**放到堆中；
- 如果变量在函数外部没有引用，则**优先**放到栈中；

#### 线程、协程

goroutine没有id,不能在一个协程中杀死另外一个协程，编码时需要考虑到协程什么时候创建，什么时候释放

协程存在于用户态，由go语言运行时调度器进行调度。协程从属于某一个线程，多个协程可以调度到一个线程中，一个协程也可能切换到多个线程中执行，因此协程与线程是多对多(M:N)的关系。

  #### sync.Map 与 map

原理：`sync.Map` 使用了一种**无锁设计**（尽量减少显式锁的使用），它主要通过原子操作和内部分离的读写存储来实现并发安全。具体而言，`sync.Map` 内部有两个主要的存储结构，一个是用于读操作的**读视图**（read-only map），另一个是用于写操作的**脏map**（dirty map）。

特点：适合读操作多，写操作少的场景；一旦一个键被存入`sync.Map`，就很少有对它的删除或更新操作。

**读视图：**有一个脏标记位，支持并发读。

**脏map：**修改时需上锁，并且标记对应的读视图





####  反射





#### channel 交替打印数字

```go
func main() {
	numberChannel := make(chan struct{})
	letterChannel := make(chan struct{})
	wait := sync.WaitGroup{}
	wait.Add(2)
	go func() {
		defer wait.Done()
		for i := 1; i <= 26; i += 2 {
			<-numberChannel
			fmt.Printf("%d%d", i, i+1)
			letterChannel <- struct{}{}
		}
	}()

	go func() {
		defer wait.Done()
		for i := 'A'; i <= 'Z'; i += 2 {
			<-letterChannel
			fmt.Printf("%c%c", i, i+1)
			if i < 'Z'-1 {
				numberChannel <- struct{}{}
			}
		}
	}()
	numberChannel <- struct{}{}
	wait.Wait()
}

```

