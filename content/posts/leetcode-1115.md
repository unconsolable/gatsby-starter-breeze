---
title: Leetcode 1115
description: 一个线程同步问题
tags:
  - Leetcode
  - 线程同步
date: "2020-12-26"
---

* 描述
  * 提供一个FooBar类, 两个不同的线程将会共用一个FooBar实例。其中一个线程将会调用foo()方法，另一个线程将会调用bar()方法。
  * 修改程序保证被foobar被输出n次
* 解法
  * pthread_cond_t结合pthread_mutex_t
  * pthread_mutex互斥进临界区
  * pthread_cond用于向另一个函数发信号
  * 参考ostep ch30的读写者实现, 使用cnt和条件变量联合判断输出是否轮到该线程执行
  ```cxx
  class FooBar {
      private:
          int n;
          pthread_cond_t fooCond,barCond;
          pthread_mutex_t mutex;
          int cnt = 0;
      public:
          FooBar(int n) {
              this->n = n;
              pthread_cond_init(&fooCond, nullptr);
              pthread_cond_init(&barCond, nullptr);
              pthread_mutex_init(&mutex, nullptr);
              pthread_cond_signal(&fooCond);
          }
      
          void foo(function<void()> printFoo) {
              
              for (int i = 0; i < n; i++) {
                  pthread_mutex_lock(&mutex);
                  while (cnt == 1)
                      pthread_cond_wait(&fooCond, &mutex);
              	// printFoo() outputs "foo". Do not change or remove this line.
              	printFoo();
                  cnt = 1;
                  pthread_cond_signal(&barCond);
                  pthread_mutex_unlock(&mutex);
              }
          }
      
          void bar(function<void()> printBar) {
              
              for (int i = 0; i < n; i++) {
                  pthread_mutex_lock(&mutex);
                  while (cnt == 0)
                      pthread_cond_wait(&barCond, &mutex);
              	// printBar() outputs "bar". Do not change or remove this line.
              	printBar();
                  cnt = 0;
                  pthread_cond_signal(&fooCond);
                  pthread_mutex_unlock(&mutex);
              }
          }
      };
  ```
