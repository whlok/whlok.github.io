---
title: Kotlin协程入门
date: 2023-10-30 10:38:46
categories: Kotlin
tags: coroutine
---

## 基础概念

## 协程

**怎么定义？**

WIKI: Coroutines are computer program components that generalize subroutines for non-preemptive multitasking, by allowing execution to be suspended and resumed. 

协程是计算机程序组件，它通过允许暂停和恢复执行来概括非抢占式多任务处理的子程序。



**协程概念最核心的点**在于：函数或者一段程序能够被挂起，稍后再在挂起的位置恢复。

挂起和恢复时开发者的程序逻辑自己控制的，协程是通过主动挂起出让运行权来实现协作，因此本质上是讨论程序控制流程的机制。



**与线程最大的差别**

- 从任务的角度，线程一旦执行就不会暂停，直到任务结束，这个过程是连续的;
- 从调度方式的角度，线程之间是抢占式的调度，因此不存在协作问题.



## 异步程序设计

**同步，异步（用线程来理解）**

- **同步**：线程自己去获取结果（一个线程）

- - 例如：线程调用一个方法后，需要等待方法返回结果

- **异步**：线程自己不去获取结果，而是由其它线程返回结果（至少两个线程）

- - 例如：线程A调用一个方法后，继续向下运行，运行结果由线程B返回

同步异步是相对于指令的执行顺序来说的，顺序执行就是同步，反之就是异步。

// Todo: 添加同步异步代码DEMO

**回调地狱：回调不断嵌套，让程序难以理解和掌控**

**// Todo: 添加回调地狱的DEMO**

### 异步程序的关键问题

**结果传递**

**异步调用的结果是立即返回的，被调用方有俩种情况**

![img](../image/kotlin/yuque_mind.jpeg)



**异常处理**

同步

```java
try{
    ...
}catch(...){
    ...
}
```



**取消响应**



取消响应中的响应是很关键的一点，需要异步任务主动配合取消，如果不配合，那么外部也就没有办法，只能听之任之。



**复杂分支**



### 常见的设计思路

**Future**

JDK 1.5引入，有一个get方法，能够同步阻塞地返回Future对应的异步任务的结果。但一旦调用其中一个get，当前调用也就被阻塞了。在所有的get返回之前，当前的调用流程会一直被限制在这段逻辑中。

通过阻塞当前调用来等待异步结果，让异步的逻辑变得不像”异步“了，是因为我们还得同步等待结果。

**CompletableFuture**

JDK 1.8新增，通过它可以拿到异步结果。与直接使用Future不同，get函数的调用仍然在CompletableFuture提供的异步调用环境中，不会阻塞主调用流程。

解决了异步结果不阻塞主调用流程的问题，但让结果的获取脱离了主调用流程。



**Promise 与async/ await**

Promise是一个异步任务，存在挂起，完成，拒绝三个状态。

- 当处于完成时状态，结果通过调用then方法的参数进行回调；
- 当出现异常拒绝时，通过catch方法传入的参数来捕获拒绝的原因。



async/await很好的兼顾了异步任务执行和同步语法结构的需求。



**Reactive Programming**

主要关注的是数据流的变换和流转，更注重数据的输入和输出之间的关系。



### koltin协程

只用一个关键字 “suspend”表示挂起点，包含了异步调用和回调俩层含义。

所有异步回调对于当前调用流程来说都是一个挂起点，在这个挂起点我们可以做的事情非常多，既可以像async/await那样异步回调，又可以添加调度器处理线程切换，还可以作为协程取消响应的位置，等。



异步逻辑同步化。



## Continuation Passing Style(CPS)

Continuation Passing Style(续体传递风格): 约定一种编程规范，函数不直接返回结果值，而是在函数最后一个参数位置传入一个 callback 函数参数，并在函数执行完成时通过 callback 来处理结果。回调函数 callback 被称为续体(Continuation)，它决定了程序接下来的行为，整个程序的逻辑通过一个个 Continuation 拼接在一起。



**Kotlin 协程本质就是利用 CPS 来实现对过程的控制，并解决了 CPS 会产生的问题(如回调地狱，栈空间占用)**

- Kotlin suspend 挂起函数写法与普通函数一样，但编译器会对 suspend 关键字的函数做 CPS 变换，用看起来同步的方式写出异步的代码，消除回调地狱(callback hell)。
- 为了避免栈空间过大的问题, Kotlin 编译器并没有把代码转换成函数回调的形式，而是利用状态机模型。每两个挂起点之间可以看为一个状态，每次进入状态机时都有一个当前的状态，然后执行该状态对应的代码；如果程序执行完毕则返回结果值，否则返回一个特殊值，表示从这个状态退出并等待下次进入。相当于创建了一个可复用的回调，每次都使用这同一个回调，根据不同状态来执行不同的代码。



```java
public interface Continuation<in T> {
    // 当前协程的 CoroutineContext 上下文
    public val context: CoroutineContext
    // 传递 result 恢复协程
    public fun resumeWith(result: Result<T>)
}
```

## 



## 创建协程

方式1:

```kotlin
fun main() {
    CoroutineScope(Job()).launch(Dispatchers.Default) {
        delay(1000L)
        println("Kotlin")
    }

    println("Hello")
    Thread.sleep(2000L)
}

//执行结果：
//Hello
//Kotlin
```

方式2:

```kotlin
fun main() = runBlocking {
    val deferred = CoroutineScope(Job()).async(Dispatchers.Default) {
        println("Hello")
        delay(1000L)
        println("Kotlin")
    }

    deferred.await()
}

//执行结果：
//Hello
//Kotlin
```

方式3:

```kotlin
fun main() {
    runBlocking(Dispatchers.Default) {                
        println("launch started!")     
        delay(1000L)          
        println("Kotlin")         	   
    }

    println("Hello")            		
    Thread.sleep(2000L)          
    println("Process end!")            
}

//执行结果：
//launch started!
//Hello
//Kotlin
```

异同：

- launch：无法获取执行结果，返回类型Job，不会阻塞；
- async：可获取执行结果，返回类型Deferred，调用await()会阻塞，不调用则不会阻塞但也无法获取执行结果；
- runBlocking：可获取执行结果，阻塞当前线程的执行，多用于Demo、测试，官方推荐只用于连接线程与协程。



## 理解协程的创建

**CoroutineScope**

**Job**

**runBlocking**

**async**

**launch**

```kotlin
fun main() {
	CoroutineScope(Job()).launch(Dispatchers.Default) {
		delay(1000L)
		println("Kotlin")
	}
	
	println("Hello")
	Thread.sleep(2000L)
}


=====>launch

public fun CoroutineScope.launch(
    // 协程的上下文,用于提供协程启动和运行时需要的信息,也可以传递Kotlin提供的Dispatchers来指定协程运行在哪一个线程中
    context: CoroutineContext = EmptyCoroutineContext,
    // 协程的启动模式
    start: CoroutineStart = CoroutineStart.DEFAULT,
	// 协程的函数体
    block: suspend CoroutineScope.() -> Unit
): Job {
    val newContext = newCoroutineContext(context)
    val coroutine = if (start.isLazy)
        LazyStandaloneCoroutine(newContext, block) else
        StandaloneCoroutine(newContext, active = true)
    // 协程的创建
    coroutine.start(start, coroutine, block)
    return coroutine
}

=====> coroutine.start(start, coroutine, block)
public abstract class AbstractCoroutine<in T>(
    parentContext: CoroutineContext,
    initParentJob: Boolean,
    active: Boolean
) : JobSupport(active), Job, Continuation<T>, CoroutineScope {

	...

    /**
     * 用给定的block和start启动协程
     * 最多调用一次
	 * @param: start-启动策略
     */
    public fun <R> start(start: CoroutineStart, receiver: R, block: suspend R.() -> T) {
        start(block, receiver, this)
    }
}

=====> CoroutineStart
public enum class CoroutineStart {

   /**
    * 根据上下文立即调度协程执行。
    */ 
    DEFAULT

   /**
    * 延迟启动协程，只在需要时才启动。
    * 如果协程[Job]在它有机会开始执行之前被取消，那么它根本不会开始执行
    * 而是以一个异常结束。
    */ 
    LAZY

   /**
    * 以一种不可取消的方式，根据其上下文安排执行的协程；
    * 类似于[DEFAULT]，但是协程在开始执行之前不能取消。
    * 协程在挂起点的可取消性取决于挂起函数的具体实现细节，如[DEFAULT]。
    */ 
    ATOMIC

   /**
    * 立即执行协程，直到它在当前线程中的第一个挂起点；
    */ 
    UNDISPATCHED：
}

public operator fun <T> invoke(block: suspend () -> T, completion: Continuation<T>): Unit =
    when (this) {
    	// 可被取消
        DEFAULT -> block.startCoroutineCancellable(completion)
        ATOMIC -> block.startCoroutine(completion)
        // 不会被分发
        UNDISPATCHED -> block.startCoroutineUndispatched(completion)
        LAZY -> Unit // will start lazily
    }

=====>startCoroutine()
/**
 * 启动一个没有接收器且结果类型为[T]的协程。
 * 每次调用这个函数时，它都会创建并启动一个新的、可挂起计算实例。
 * 当协程以一个结果或一个异常完成时，将调用[completion]延续。
 */
public fun <T> (suspend () -> T).startCoroutine(completion: Continuation<T) {
    createCoroutineUnintercepted(completion).intercepted().resume(Unit)
}

=======>createCoroutineUnintercepted
public expect fun <T> (suspend () -> T).createCoroutineUnintercepted(
    completion: Continuation<T>
): Continuation<Unit>

// expect 表示期望在具体的平台中实现。

// JVM :: IntrinsicsJvm.kt
//actual代表的是createCoroutineUnintercepted在JVM平台上的具体实现      
public actual fun <T> (suspend () -> T).createCoroutineUnintercepted(
    completion: Continuation<T>
): Continuation<Unit> {
    val probeCompletion = probeCoroutineCreated(completion)
    return if (this is BaseContinuationImpl)
        create(probeCompletion)
    else
        createCoroutineFromSuspendFunction(probeCompletion) {
            (this as Function1<Continuation<T>, Any?>).invoke(it)
        }
}
```



## 参考链接

https://www.bennyhuo.com/project/kotlin-coroutines.html

[Kotlin 官方文档 中文版](https://book.kotlincn.net/kotlincn-docs.pdf)

https://juejin.cn/post/7142743424670629895

https://juejin.cn/post/7137905800504148004
