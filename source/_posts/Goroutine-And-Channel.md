---
title: Goroutine & Channel
date: 2019-10-24 09:28:31
tags:
---

# 并发模型

##  进程 vs 线程 vs Goroutine

* 进程，是操作系统分配资源的基本单元。不同的进程之间内存空间资源独占，只能通过信号、管道、文件等方式进行通信。PHP-FPM即采取多进程并发模型，每一个请求过来，都会fork一个独立的进程用于处理该请求。
* 线程，是操作系统调度的基本单元。同一进程下的不同线程之间共享内存，可能出现资源竞争等问题。Java Servlet即采用多线程并发模型，每一个请求过来，都会创建一个独立的线程用于处理该请求。由于多线程使用共同的内存空间，就需要考虑全局性资源（全局的变量、对象、文件等）的线程安全问题。
* Goroutine，是一种协程，即用户空间的线程，操作系统不直接调度。Golang使用CSP模型实现并发，goroutine和channel即分别对应CSP模型中的Process和Channel。不同于多线程并发模型需要在竞态情形(race condition)下，通过复杂的锁机制确保资源正确使用。goroutine之间可以使用channel进行通信。Channel可以看成一个 FIFO 队列，对 FIFO 队列的读写都是原子的操作，不需要加锁。

# Goroutine & Channel

## 基本操作

```golang
channel := make(chan int) // 创建通道

go fun(){                 // 开启新协程
    channel <- 1          // 向通道发送数据
}()

result:= <-channel        // 接受通道中的数据
fmt.Println(result)
```