---
layout:     post   				    # 使用的布局（不需要改）
title:      Golang Low Level Programming 				# 标题
subtitle:   unsafe package #副标题
date:       2019-10-24 				# 时间
author:     frankie 						# 作者
header-img: img/golang_unsafe.png 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - Golang
    - unsafe
    -
---

# Ordinary Golang Programming
* Compilation: static type checking

* Dynamic checks during run time (e.g. out of bounds)

* Automatic memory management (garbage collection) (e.g. eliminating memory leaks)

* Many implementation details inaccessible to Go programs (e.g. memory layout of an aggregate type like a struct)

* The Go scheduler transparently moves goroutines from one thread to another (M:N scheduler)

* Encapsulated variable numeric address (e.g. Address may change as the garbage collector moves variables; pointers are transparently updated)

Sometimes, in order to achieve the highest possible performance, the rules are forfeited.

# unsafe package & cgo
* cgo: Go bindings for C libraries and operating system calls

* dependency introduced on specific implementation

* unsafe: implmented by compiler and expose details of Go's memory layout

* used for runtime, os. syscall and net that interact with OS

* uintptr: its width is not specified but is sufficient to hold all the bits of a pointer value. The uintptr type is used only for low-level programming, such as at the boundary of a Go program wit h a C library or an operating system (unsigned integer wide enough to represent an address)

## Sizeof
* unsafe.Sizeof: reporting the size in bytes of the representation of its operand, which may be an expression of any type; the expression is not evaluated.
```
import "unsafe"
fmt.Println(unsafe.Sizeof(float64(0))) // "8"
```

* Sizeof reports only the size of the fixed part of each data structure, like the pointer and length of a string , but not indirect parts like the contents of the string.

* memory alignment: improving addressing efficiency
* word : 4 bytes on a 32-bit platform and 8 bytes on a 64-bit platform

```
Type                            Size

bool                            1 byte
intN, uintN, floatN, complexN   N / 8 bytes (for example, float64 is 8 bytes)
int, uint, uintptr              1 word
*T                              1 word
string                          2 words (data, len)
[]T                             3 words (data, len, cap)
map                             1 word
func                            1 word
chan                            1 word
interface                       2 words (type, value)
```

## Alignof
* The unsafe.Alignof function reports the required alignment of its argument’s type

* Typically, boolean and numeric types are aligned to their size (up to a maximum of 8 bytes) and all other types are word-aligned.


## Offsetof
* The unsafe.Offsetof function, whose operand must be a field selector x.f, computes the offset of field f relative to the start of its enclosing struct x, accounting for memory holes, if any.

```
var x struct {
  a bool
  b int16
  c []int
}
```

![Imgur](https://i.imgur.com/OwqDOuR.png)

```
Typical           32-bit platform:
Sizeof(x)  = 16   Alignof(x) = 4
Sizeof(x.a) = 1   Alignof(x.a) = 1  Offsetof(x.a) = 0
Sizeof(x.b) = 2   Alignof(x.b) = 2  Offsetof(x.b) = 2
Sizeof(x.c) = 12  Alignof(x.c) = 4  Offsetof(x.c) = 4

Typical           64-bit platform:
Sizeof(x)  = 32   Alignof(x) = 8
Sizeof(x.a) = 1   Alignof(x.a) = 1  Offsetof(x.a) = 0
Sizeof(x.b) = 2   Alignof(x.b) = 2  Offsetof(x.b) = 2
Sizeof(x.c) = 24  Alignof(x.c) = 8  Offsetof(x.c) = 8
```

# Pointer
* The unsafe.Pointer type is a special kind of pointer that can hold the address of any variable

* unsafe.Pointers are comparable and may be compare d with nil

* (*unsafe.Pointer) can not be indirected through, because we don't know what type that expression should have

```
package math
func Float64bits(f float64) uint64 { return *(*uint64)(unsafe.Pointer(&f)) }
fmt.Printf("%#016x\n", Float64bits(1.0)) // "0x3ff0000000000000"
```

* uintptr -> unsafe.Pointer may not work because not all numbers are valid addresses

```
var x struct {
  a bool
  b int16
  c []int
}

// equivalent to pb := &x.b
pb := (*int16)(unsafe.Pointer(
    uintptr(unsafe.Pointer(&x)) + unsafe.Offsetof(x.b)))

*pb = 42

fmt.Println(x.b) // "42"

// Try to replace (* int16) to (* float64) and observe the result again -> 0
```


* Diff between uintptr & unsafe.Pointer

GC may move the variables around in memory to reduce fragmentation or bookkeeping (moving GCs). When this happens, all pointers that hold the address of the old location must be updated to point to the new one. GC takes unsafe,Pointer as an pointer and thus its value must change as the variables moves, but unintptr is just a number so its value must not change.
```
// NOTE: subtly incorrect!
tmp := uintptr(unsafe.Pointer(&x)) + unsafe.Offsetof(x.b)
pb := (*int16)(unsafe.Pointer(tmp))
*pb = 42
```

There are no pointers that refer to the variable create d by new, so the garbage collector is entitled to recycle its storage when this statement completes, after which pT contains the address where the variable was but is no logger.

```
pT := uintptr(unsafe.Pointer(new (T))) // NOTE: wrong!
```

* No guarantees after an unsafe.Pointer -> uintptr conversion
minimize the number of operations between converting an unsafe.Pointer to uintptr and using that uintptr

* When cal ling a library function that returns a uintptr, such as those below fro m the reflect package, the result should be immediately converted to an unsafe.Pointer to ensure that it continues to point to the same variable.

```
package reflect
func (Value) Pointer() uintptr
func (Value) UnsafeAddr() uintptr
func (Value) InterfaceData() [2]uintptr // (index 1)
```
