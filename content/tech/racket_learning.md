---
title: "Racket_learning"
date: 2023-01-11T13:36:27+08:00
draft: false
---

## 1. Intro
Racket是 [Scheme](http://zh.wikipedia.org/wiki/Scheme) 的一种方言，而Scheme又是 [Lisp](http://zh.wikipedia.org/zh-cn/LISP) 的一种方言。作为一门古老的计算机语言，Lisp一直被许多人视为史上最非凡的编程语言。作为早期的高端编程语言之一，Lisp开创了计算机科学领域的许多概念，包括树结构、自动存储器管理、动态类型、条件表达式、高端函数、递归、自主编译器、读取﹣求值﹣输出循环（REPL）...

**Racket**包含了几乎所有Lisp的优点，同时也提供了大量有用的库，降低初学者学习的成本。

[Racket下载](https://download.racket-lang.org/)

运行racket代码的方式：
1. 使用REPL交互式执行，例如：“racket”
2. 将代码写入.rkt后缀的文件中，racket xxx.rkt

IDE/编辑器选择：
1. vscode + magic racket extension
2. DrRacket（racket自带ide）

## 2. Examples

不同于其他语言，racket（也就是scheme/lisp语言）是基于前缀表达式的
```scheme
#lang racket

(+ 1 1) ; 2
(/ 6 4) ; 3/2
(expt 2 3) ; 8, 次方
(quotient 5 2) ; 2, 商
(remainder 8 3) ; 2, 余数

(not #t) ; #f, 非 [#t为true，#f为false]
(and 1 #f) ; #f, 与 [任何不是#f的参数均视为#t]
(and -1 2) ; 2, [and的结果在为#t时，会返回一个比#t更有意义的结果]
(or -1 #f) ; -1, 或
(xor #f 3) ; 3, 异或

(> 1 2) ; #f
(= 1 1) ; #t

(string-append "hello" " " "world") ; "hello world", 字符串拼接
(format "~a, ~a" "hello" "world") ; "hello, world", 格式化
(printf "~a, ~a" "hello" "world") ; "hello, world", 格式化输出

; "->"为转换
(string->number "23") ; 23, 字符串转数字
(number->string 23) ; "23"

(string-length "hello") ; 5, 字符串长度 [基于unicode]

; "?"为判断
(string? "hi") ; #t, 是否为字符串
(number? "s") ; #f, 是否为数字

; 定义变量

(define a 100)
(format "~a" a) ; 100

; 定义函数
(define
    (add a b) ; 函数名，参数列表
    (+ a b)) ; 函数体
(add 1 2) ; 3

; 引入库
(require 2htdp/image)
(define flag (rectangle 100 61.8 "solid" "red"))
(define a_star (star 15 "solid" "yellow"))
(overlay a_star flag)
```

## 3. Basics

### 3.1 Grammar
#### 3.1.1 variable

变量绑定：
```scheme 
; define可以全局绑定，但是每次只能绑定一个变量。
(define x 1)

; 使用let可以一次绑定多个变量，但只在局部作用域内有效
(let ([a 3]
      [b 4])
    (* a b))

; 一次绑定多个值
(let-values ([(a b) (quotient/remainder 10 3)])
  (+ a b))
```

#### 3.1.2 condition

if语句
```scheme
> (if test-expr true-expr false-expr) → value
  test-expr : (判断条件)
  true-expr : (当条件为真时执行的表达式)
  false-expr : (当条件为假时执行的表达式)
  
; if
(if (positive? -1) (error "impossible") 666)
(if "hhh" "yes" "no") ; 任何不为#f的均视作#t

; if 包含分支
(define z 55)
(if (> z 90) "A"
    (if (> z 70) "B"
        (if (> z 60) "C"
            "Not Pass")))
```
condition语句
```scheme
; condition
(cond [(> z 90) "A"]
      [(> z 70) "B"]
      [(> z 60) "C"]
      [else "Not Pass"])
```
#### 3.1.3 loop and recursion

	当存在多个 clause 时，不带 * 的版本会同时处理每个 sequence-expr，并且，只要任何一个 sequence-expr 结束，循环便结束；而带 * 的版本会嵌套处理每个 sequence-expr，直至全部可能都被穷尽。

```scheme
; for，返回值为list
(for/list ([i '(1 2 3 4)]
           [name '("goodbye" "farewell" "so long")])
    (format "~a: ~a" i name))
    
; '("1: goodbye" "2: farewell" "3: so long")

(for*/list ([i '(1 2 3 4)]
           [name '("goodbye" "farewell" "so long")])
    (format "~a: ~a" i name))
    
  ; '("1: goodbye"
  ; "1: farewell"
  ; "1: so long"
  ; "2: goodbye"
  ; "2: farewell"
  ; "2: so long"
  ; "3: goodbye"
  ; "3: farewell"
  ; "3: so long"
  ; "4: goodbye"
  ; "4: farewell"
  ; "4: so long")
```

```scheme
; for/product，相乘
(for/product ([i '(1 2 3)]
              [j '(4 5 6)])
  (* i j))

; for/sum，相加
(for/sum ([i '(1 2 3)]
          [j '(4 5 6)])
  (* i j))

; for/last，最后一个值
(for/last ([i '(1 2 3)]
           [j '(4 5 6)])
  (* i j))

; for/hash，哈希
(for/hash ([i '(1 2 3)]
           [j '(4 5 6)])
(values i j))

(for/list ([i (in-range 10)])
  (sqr i))

...
```

### 3.2 Data Structure


