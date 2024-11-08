https://www.csview.cn/cpp/

## 基本语法

==布尔型无法参与数值运算，也无法与其他类型进行转换==

### **<font color='red'>短变量和var的区别</font>**

`:=` 更简洁，但仅限局部作用域且必须赋初值。

`var` 更灵活，可用于全局或局部作用域，且可以声明不赋初值

`if` 支持初始化语句，用分号分隔

### **<font color='red'>反射的优点和缺点</font>**

在 Go 语言中，**反射**是一种在运行时动态操作变量类型和值的机制，主要通过 `reflect` 包来实现。反射允许我们在不知道变量实际类型的情况下进行操作，这在编写通用代码时非常有用。

在 Go 中，反射的核心类型是 `reflect.Type` 和 `reflect.Value`：

- **`reflect.Type`**：提供变量的类型信息。
- **`reflect.Value`**：提供变量的值信息，允许在运行时修改其值（如果类型是可设置的）。

通常使用 `reflect.TypeOf()` 和 `reflect.ValueOf()` 来获取变量的 `Type` 和 `Value`，并进行相应操作。

```go
func main() {
	var x float64 = 3.4

	v := reflect.ValueOf(x)
	fmt.Println("Type:", v.Type())               // 获取类型
	fmt.Println("Kind is float64:", v.Kind())    // 获取种类
	fmt.Println("Value:", v.Float())             // 获取值

	// 如果需要设置值，需要使用 reflect.ValueOf(&x).Elem()
	v = reflect.ValueOf(&x).Elem()
	v.SetFloat(7.1)
	fmt.Println("Updated Value:", x)             // 更新后的值
}
```

==反射的优点==

**动态性强**：反射可以在运行时操作类型不确定的数据，使代码更加灵活和通用。

**实现通用函数**：反射允许编写可以处理不同数据类型的通用函数，比如序列化、数据映射等功能，适合需要对多种类型数据进行操作的场景。

**简化编码**：在一些需要操作未知类型或结构的场景中，比如处理 JSON 数据，反射可以减少手动编码的工作量。

==反射的缺点==

**性能开销**：反射在运行时动态解析类型和操作变量，其执行效率远低于普通的直接调用。因此在性能敏感的场合，应尽量避免过多使用反射。

**类型安全性差**：反射跳过了编译时的类型检查，可能会在运行时产生错误（例如 `nil` 值或类型断言失败），难以在编译时发现问题。

**代码可读性降低**：由于反射代码较为抽象和不直观，理解和调试代码会更复杂，增加了代码维护的难度。

### **<font color='red'>为什么实例化结构体时要使用指针</font>**

减少内存占用、避免结构体拷贝和实现 `nil` 值判断

### **<font color='red'>接口</font>**

**实现多态**：不同类型可以实现同一接口，从而让接口变量可以指向不同的具体类型，实现多态性

**松耦合设计**：让代码依赖于抽象行为，而非具体实现

**支持面向接口编程**：编写更通用、复用性更高的代码

实例可以强制类型转换为接口，接口也可以强制类型转换为实例

```go
func main() {
	var p Person = &Student{
		name: "Tom",
		age:  18,
	}
	stu := p.(*Student) // 接口转为实例
	fmt.Println(stu.getAge())
}
```

如果定义了一个没有任何方法的空接口，那么这个接口可以表示任意类型

==任何类型的值都可以赋值给空接口==

```go
func main() {
	m := make(map[string]interface{})
	m["name"] = "Tom"
	m["age"] = 18
	m["scores"] = [3]int{98, 99, 85}
	fmt.Println(m) // map[age:18 name:Tom scores:[98 99 85]]
}
```

Go 语言支持通过**接口断言**将接口类型转换为具体的对象类型。接口断言的语法如下：

```go
value, ok := interface.(Type)
```

- `interface` 是一个接口类型的变量。
- `(Type)` 是要断言的具体类型。
- `ok` 是一个布尔值，表示断言是否成功。如果成功，`value` 是转换后的具体类型值；如果失败，`ok` 为 `false`。

Go 语言也有 Public 和 Private 的概念，粒度是包。如果类型/接口/方法/函数/字段的首字母大写，则是 Public 

的，对其他 package 可见，如果首字母小写，则是 Private 的，对其他 package 不可见

### **<font color='red'>C++和Go有什么区别</font>**

C++通过线程和锁来处理并发， ==Goroutines和channels==是Go语⾔的并发特性的核⼼ 

C++⽀持==⾯向对象编程==等⾼级特性

Go语⾔具有==垃圾回收==机制，开发者⽆需⼿动管理内存 

### **<font color='red'>string</font>**

- 字符串的长度可以使用 len() 函数获取，它返回的是字符串的字节数
- 字符串可以使用索引访问单个字节

```go
str := "世界"
// 将字符串转换为 rune 切片
runes := []rune(str)
// 访问第一个字符 "世"
fmt.Println(string(runes[0])) // 输出: 世
fmt.Println(string(str[0:3])) // 输出: 世
```

- 要修改字符串，通常需要创建一个新的字符串

**`string`**：表示不可变的字节序列，主要用于处理文本数据，其内容不能被直接修改。

**`[]byte`**：表示可变的字节序列，主要用于处理二进制数据，需要频繁修改的字符串。

**转换**：可以在 `string` 和 `[]byte` 之间相互转换，会创建新的底层数组，但要注意性能问题，避免不必要的转换操作。

```go
s := "hello"
b := []byte(s)
fmt.Println(b)                  // 输出：[104 101 108 108 111]
```

```go
b := []byte{'h', 'e', 'l', 'l', 'o'}
s := string(b)
fmt.Println(s)                  // 输出：hello
```

### **<font color='red'>字符串转成byte数组，会发生内存拷贝吗？</font>**

严格来说，只要是发生类型强转都会发生内存拷贝

```go
func main() {
 a :="aaa"
 ssh := *(*reflect.StringHeader)(unsafe.Pointer(&a))
 b := *(*[]byte)(unsafe.Pointer(&ssh))  
 fmt.Printf("%v",b)
}
```

`StringHeader` 是`字符串`在go的底层结构

```go
type StringHeader struct {
 Data uintptr
 Len  int
}
```

`SliceHeader` 是`切片`在go的底层结构

```go
type SliceHeader struct {
 Data uintptr
 Len  int
 Cap  int
}
```

- 1.`unsafe.Pointer(&a)`方法可以得到变量`a`的地址。
- 2.`(*reflect.StringHeader)(unsafe.Pointer(&a))` 可以把字符串a转成底层结构的形式。
- 3.`(*[]byte)(unsafe.Pointer(&ssh))` 可以把ssh底层结构体转成byte的切片的指针。
- 4.再通过 `*`转为指针指向的实际内容。

```go
func BytesToStr(data []byte) string {
	return *(*string)(unsafe.Pointer(&data))
}
func StrToBytes(data string) []byte {
	str := (*reflect.StringHeader)(unsafe.Pointer(&data))
	bs := reflect.SliceHeader{
		Data: str.Data,
		Len:  str.Len,
		Cap:  str.Len,
	}

	return *(*[]byte)(unsafe.Pointer(&bs))
}
```

### **<font color='red'>Go和C++字符串的区别？</font>**

1、Go语言字符串和C++中string有所不同，其中最大的不同就是Go语言中字符串无法直接通过下标修改每一个字

符元素，只能通过重新构造新的字符串并赋值给原来的字符串变量实现

2、Go语言的字符有以下两种：

一种是uint8类型，或者叫byte类型，代表了一个ASCII码的一个字符。等价于C语言中unsigned char

一种是rune类型，代表UTF-8字符，底层是int32类型。用来处理中文、日文等复合字符。

**<font color='red'>翻转含有中文、数字、英文字母的字符串</font>**

```go
func main() {
  src := "你好abc啊哈哈"
  dst := reverse([]rune(src))
}

func reverse(s []rune) []rune {
  for i, j := 0, len(s)-1; i < j; i++, j-- {
  s[i], s[j] = s[j], s[i]
  }
  return s
}
```

`rune`关键字，它是int32的别名（-2^31 ~ 2^31-1），比起byte（-128～127），可表示更多的字符

### **<font color='red'>那Go语言如何实现C++中的引用效果呢</font>**

**指针**是 Go 语言实现 C++ 引用效果的主要手段

通过传递指针，函数可以直接操作变量的内存地址，达到修改原始变量的效果

Go 中的引用类型（如切片和 map）天然具有类似引用的行为，不需要显式使用指针

### **<font color='red'>make和new的区别</font>**

- `new(T)`：分配内存但不进行初始化，只返回指向类型`T`的指针

- `make(T, args...)`：用于slice、map和channel的内存分配和初始化，返回的是已经初始化好的值而非指针

**<font color='purple'>能不能new一下map？</font>**

- new后的是nil，不能使用，只能创建一个map赋值给它

直接使用 `map` 作为函数参数已经能满足大部分需求，因为 `map` 是引用类型。只有在你明确需要在函数内重新初始化或更换整个 `map` 实例时，才会考虑传递指针来实现

```go
// 使用指针传递 map，这样可以在函数内更改整个 map 的实例
func modifyMap(m **map[string]int) {
    // 重新初始化 map
    *m = make(map[string]int)

    // 添加新元素
    (*m)["grape"] = 10
    (*m)["melon"] = 15
}
func main() {
    // 使用 new 创建 map 指针
    myMap := new(map[string]int)
    // 初始化 map
    *myMap = make(map[string]int)
    (*myMap)["apple"] = 5
    (*myMap)["banana"] = 2
    fmt.Println("Before modifying:", *myMap)
    // 调用函数，修改 map
    modifyMap(&myMap)
    fmt.Println("After modifying:", *myMap)
}
```

**传递 `map` 本身（不使用指针）**：可以在函数中修改 `map` 的内容，但不能改变其引用。更改 `map` 的键值对会反映在调用者中，而重新分配一个新的 `map` 不会生效。

**传递 `map` 指针**：不仅可以修改内容，还可以在函数内重新分配 `map` 实例，并使调用者能够看到这个更改。

### **<font color='red'>struct能不能比较？</font>**

不同类型的结构体，如果成员变量类型、变量名和顺序都相同，而且结构体没有不可比较字段时，那么进行显式类型转换后就可以比较，反之不能比较。

同类型的struct分为两种情况：

- struct的所有成员都是可以比较的，则该strcut的不同实例可以比较

- struct中含有不可比较的成员，则该struct不可以比较

当需要比较两个struct内容时，最好使用reflect.DeepEqual方法进行比较，无论什么类型均可满足比较要求。

==不可比较的类型==

- slice，因为slice是引用类型，除非和nil比较：因为slice的元素是间接引用的，一个slice甚至可以包含自身，slice的变量实际是一个指针，使用 == 其实在判断地址
- map，和slice同理，如果要比较两个map只能通过循环遍历实现
- 函数类型，不能比较

**<font color='red'>类型之间怎么强制类型转换</font>**

**基础类型转换**使用显式的 `T(x)` 语法。

**接口类型转换**使用**类型断言**来从接口转换为具体类型，或者在接口之间转换。

### **<font color='red'>Go语言中引用传递的数据结构</font>**

**`map`**：传递的是底层哈希表的引用。

**`slice`**：传递的是底层数组的引用和相关元数据（长度和容量）。

**`channel`**：传递的是底层队列的引用。

**`interface`**：接口类型本身是一个引用类型。

**`function`**：函数本身是一个引用，可以将函数作为参数或返回值。

### **<font color='red'>介绍⼀下panic和recover</font>**

panic 和 recover 是⽤于处理运⾏时错误和恢复程序执⾏的两个关键字。但是在⼀般情况下，Go语⾔更倾向于 使

⽤显式的错误处理，==⽽不是依赖于 panic 和 recover==

==panic：==

1、panic 是⼀个内建函数，⽤于引发运⾏时错误，通常表示程序遇到了不可恢复的错误。 

2、当程序执⾏到 panic 语句时，它会⽴即停⽌当前函数的执⾏，并沿着函数调⽤栈向上搜寻，执⾏每个被调⽤ 函

数的 defer 延迟函数（如果有的话），然后程序终⽌。 

3、panic 通常⽤于表示程序遇到了⼀些致命错误，例如切⽚越界、除以零等。

==recover：==

1、recover 是⼀个内建函数，⽤于从 panic 引发的运⾏时错误中进⾏恢复。

2、recover 只能在 defer 延迟函数中使⽤，⽤于捕获 panic 的值，并防⽌程序因 panic ⽽崩溃。 

3、如果在 defer 函数中调⽤了 recover ，并且程序处于 panic 状态，那么 recover 将返回 panic 的值， 并且程序

会从 panic 的地⽅继续执⾏

```go
// example 函数演示了在 Go 中使用 panic 和 recover 来处理异常。
func example() {
    // 使用 defer 延迟执行匿名函数，用于捕获 panic，并在发生 panic 时进行恢复。
    defer func() {
        // 使用 recover 函数捕获 panic，并将 panic 的值赋给变量 r。
        if r := recover(); r != nil {
            // 如果捕获到了 panic，则打印恢复的信息。
            fmt.Println("Recovered:", r)
        }
    }()
    // 手动触发 panic，模拟程序中出现异常情况。
    panic("Something went wrong!")
}
```

### **<font color='red'>Go⾯向对象是怎么实现的？</font>**

封装：包为粒度大小写

继承：结构体复用

多态：接口

**<font color='red'>什么是defer？有什么作⽤</font>**

后进先出（LIFO）的顺序执⾏的

defer语句中的函数参数会立即求值，但实际执行被延迟到上层函数返回之后

```go
for i := 0; i < 5; i++ {
 defer func() {
 fmt.Println(i)
 }()
}

//输出5个5
```

`defer` 在 `for` 循环中捕获的是 `i` 的引用，而不是每次循环时的值。

因此，当 `defer` 中的匿名函数执行时，它们都会打印出 `i` 的最终值，即 5。

**<font color='red'>defer的使用场景？</font>**

**并发处理**：defer wg.Done()

**锁场景**：defer mu.RUnlock()

**资源释放**：defer file.Close()

**panic-recover**：

```go
defer func() {
if v := recover(); v != nil {
  _ = fmt.Errorf("PANIC=%v", v)
}
}()
```

## Go底层原理

### map

关键点：哈希函数，存储桶（链表/红黑树处理哈希冲突），扩容机制，并发使用（互斥锁/sync.Map)

使用 `LoadOrStore` 方法，如果键已存在则返回其对应的值和 true，否则存储新的键值对并返回新值和 false

```go
value, loaded := m.LoadOrStore("key2", "new_value")
```

**<font color='red'>Go语⾔map是有序的还是⽆序的, 为什么</font>**

散列表通过哈希函数将键映射到存储桶（bucket） ，散列表中的存储桶是⽆序的，它们并不保证元素按照特定顺

序存储，每个存储桶存储⼀个链表或红⿊树，⽤于处理哈希冲突。存储桶的数量是固定的，由 ==map 的⼤⼩和负载==

==因⼦==来确定

**<font color='red'>如何想要按照特定顺序遍历map,怎么做</font>**

使用切片排序

```go
myMap := map[string]int{
    "apple": 5,
    "banana": 3,
    "orange": 7,
    "grape": 1,
}
// 将键存储在切⽚中
keys := make([]string, 0, len(myMap))
for key := range myMap {
    keys = append(keys, key)
}
// 对切⽚进⾏排序
sort.Strings(keys)
// 按照排序后的顺序遍历map
for _, key := range keys {
    fmt.Printf("%s: %d\n", key, myMap[key])
}
```

**<font color='red'>map如何并发安全</font>**

**map 的并发问题**：可以使用 `sync.Mutex` 或 `sync.Map` 来解决 `map` 的并发访问问题。

```go
// 创建⼀个并发安全的 map
var myMap sync.Map
// 在 Goroutine 中使⽤
go func() {
 // 存⼊数据
 myMap.Store("key", "value")
 // 从 map 中读取数据
 if value, ok := myMap.Load("key"); ok {
      // 处理 value
 }
}()
```

**<font color='red'>map之间如何比较</font>**

```go
func equal(x, y map[string]int) bool {
    if len(x) != len(y) {
        return false
    }
    for k, xv := range x {
        if yv, ok := y[k]; !ok || yv != xv {
            return false
        }
    }
    return true 
}
//map不能顺序读取，是因为他是无序的，想要有序读取，需要把key变为有序，所以可以把key放入切片，对切片进行排序，遍历切片，通过key取值。
```

**<font color='red'>map删除一个元素，会导致这个map占用的内存发生变化吗</font>**

 Go 中删除 `map` 的元素不会马上减少内存占用，需要依赖垃圾回收来回收未使用的内存空间

```
Before deletion:
Alloc = 62171 KiB	TotalAlloc = 85988 KiB	Sys = 93908 KiB	NumGC = 5
After deletion:
Alloc = 62172 KiB	TotalAlloc = 85988 KiB	Sys = 93908 KiB	NumGC = 5
After GC:
Alloc = 356 KiB	TotalAlloc = 85993 KiB	Sys = 93908 KiB	NumGC = 6
```

**<font color='red'>不用锁怎么实现map并发安全</font>**

**CAS**：通过原子操作（如 Compare-And-Swap）来避免锁

**分段锁/分片**：通过将 map 分片来减少锁的冲突和竞争

**Copy-on-Write**：适用于读多写少的场景，通过副本实现并发读写

**<font color='red'>如果要实现一个线程安全的哈希表。就是支持多个线程去并发读写的话，怎么实现？</font>**

粗粒度锁：最简单的实现方式是使用一个全局锁保护整个哈希表的访问，即每次读写操作都必须先获取该锁。

分段锁：为了提高并发性，可以将哈希表分成多个段，每个段独立使用一个锁

无锁哈希表：无锁哈希表通过原子操作（如 `std::atomic`）实现读写操作，不需要使用互斥锁

读写锁：如果哈希表大部分操作是读取，可以使用**读写锁**，即读操作获取共享锁，写操作获取独占锁

### slice

方式一：从数组生成切片

方式二：从切片生成切片

方式三：直接声明新的切片

方式四：使用make()函数构造切片

增

```go
var slice[] int
slice = append(slice, 3)        //使用append()一次添加一个元素
slice = append(slice, 1, 2, 3)  //使用append()一次添加多个元素
```

删：Go没有提供直接对切片删除元素的接口，只能通过append实现

切片复制

```go
//切片复制
src := []int{1, 2, 3, 4, 5}
dst := make([]int, len(src)) // 创建一个目标切片，长度与源切片相同
copy(dst, src)               // 将源切片的内容复制到目标切片
```

**<font color='red'>数组和切⽚的区别</font>**

- 长度是否可变
- 数组传递时会复制整个数组，切片传递时复制的是切片结构本身，共享底层数组

**<font color='red'>切片实现栈和队列</font>**

**<font color='red'>切⽚是如何扩容的</font>**

在 Go 中，当 `slice` 容量不足时，会触发扩容，Go 会按照以下规则创建新数组：

- 容量不足时，直接创建一个新数组，并将旧数据复制过来。
- 扩容倍数根据当前容量调整：容量小于 `512` 时按 `2 倍`扩展，达到或超过 `512` 时按 `1.3 倍`扩展。
- 一次追加多个元素时，会直接分配足够大的新数组以满足容量需求。

**<font color='red'>Go slice底层实现</font>**

切⽚本身并不是动态数组或者数组指针。它内部实现的数据结构通过指针引⽤底层数组

```go
type slice struct {
    array unsafe.Pointer // 指向底层数组的指针
    len int // 切⽚的当前⻓度
    cap int // 切⽚的容量
}
```

**<font color='red'>funcA向funcB传入slice，funcB中往slice append元素并返回，funcA中打印该slice会受影响吗</font>**

如果 `append` 不需要扩容：`funcA` 中的 `slice` 会直接反映 `funcB` 的修改。

如果 `append` 需要扩容：`funcB` 创建一个新的底层数组。`funcA` 中的原始 `slice` 不受影响，除非 `funcA` 接收并使用 `funcB` 返回的新 `slice`。

**<font color='red'>拷贝大切片一定比小切片代价大吗？</font>**

并不是，所有切片的大小相同；

三个字段:切片中的第一个字是指向切片底层数组的指针，这是切片的存储空间，第二个字段是切片的长度，第三个字段是容量。将一个 slice 变量分配给另一个变量只会复制三个机器字;

所以 拷贝大切片跟小切片的代价应该是一样的。

**<font color='red'>nil切片和空切片指向的地址一样吗？</font>**

切片的数据结构

```go
type SliceHeader struct {
 Data uintptr  //引用数组指针地址
 Len  int     // 切片的目前使用长度
 Cap  int     // 切片的容量
}
```

- nil切片和空切片指向的地址不一样。
- nil空切片引用数组指针地址为0（无指向任何实际地址），所有的空切片指向的数组引用地址都是一样的，且固定为一个值

**<font color='red'>Go 中如何处理 `nil` 切片与空切片的区别？</font>**

在 Go 中，可以通过 `==` 运算符比较切片对象。如果是 `nil` 切片，它会与 `nil` 值比较，而空切片会与一个空的底层数组比较。

```go
package main

import "fmt"

func main() {
    var nilSlice []int    // nil切片
    emptySlice := []int{} // 空切片

    fmt.Println(nilSlice == nil)   // 输出: true
    fmt.Println(emptySlice == nil) // 输出: false
}
```

**<font color='red'>for循环里面能append吗</font>**

```go
func main() {
 s := []int{1,2,3,4,5}
 for _, v:=range s {
  s =append(s, v)
  fmt.Printf("len(s)=%v\n",len(s))
 }
}
```

**不会死循环**，`for range`其实是`golang`的`语法糖`，在循环开始前会获取切片的长度 `len(切片)`，然后再执行`len(切片)`次数的循环。

### channel

**<font color='red'>channel底层原理</font>**

==结构==

**队列**：`channel` 实际上是一个先进先出（FIFO）的队列，用于存储传递的数据元素

**锁机制**：`channel` 的操作是并发安全的，底层实现使用了互斥锁（mutex）或者类似的同步机制来保证多个 goroutine 对 `channel` 的安全访问

**指针**：`channel` 内部会维护发送和接收操作的状态信息，通常使用指针来表示队列的头部和尾部。

**<font color='red'>channel发送数据和接收数据的过程？</font>**

**发送**：向通道发送数据时，如果有 Goroutine 正在等待接收，则直接将数据传递给接收者。如果没有，则将数据放入环形队列中，或阻塞等待。

**接收**：从通道接收数据时，如果有数据在队列中，则直接取出。如果没有数据但有 Goroutine 等待发送数据，则接收数据并唤醒发送者。如果通道为空，则阻塞等待。

**<font color='red'>channel的应用场景？</font>**

**控制并发数**

1、数据传递： 主要⽤于在goroutines之间传递数据，确保数据的安全传递和同步

2、同步执⾏： 通过Channel可以实现在不同goroutines之间的同步执⾏

3、消息传递： 适⽤于实现发布-订阅模型或通过消息进⾏事件通知的场景

4、多路复⽤： 使⽤ select 语句，可以在多个Channel操作中选择⼀个⾮阻塞的执⾏，实现多路复⽤

5、**控制并发数**

6、**任务定时**

```go
select {
    case <-time.After(time.Second)
}
```

**<font color='red'>channel存在3种状态</font>**

nil，未初始化的状态，只进行了声明，或者手动赋值为nil 

active，正常的channel，可读或者可写 

closed，已关闭，千万不要误认为关闭channel后，channel的值是nil

channel特性：

1. 给一个 nil channel 发送数据，造成永远阻塞
2. 从一个 nil channel 接收数据，造成永远阻塞
3. 给一个已经关闭的 channel 发送数据，引起 panic
4. 从一个已经关闭的 channel 接收数据，如果缓冲区中为空，则返回一个零值 
5. 关闭一个 nil channel 将会发生 panic

如何判断其已经被关闭

```go
if v, ok := <-ch; !ok {
	fmt.Println("channel 已关闭，读取不到数据")
}
```

**<font color='red'>channel怎么实现并发</font>**

**阻塞同步**：当一个 `goroutine` 向 `channel` 发送数据时，另一个 `goroutine` 必须从该 `channel` 接收数据，否则发送方会阻塞，反之亦然。这种机制确保了协程间的数据传递是同步的。

**无锁并发**：`channel` 可以避免共享内存带来的数据竞争问题。通过数据传递而不是共享数据来进行协作，确保并发任务的安全。

**缓冲通道**：带缓冲的 `channel` 可以暂存数据，发送方在缓冲未满时不会阻塞，这样可以提升并发效率。

**多路复用（select）**：`select` 语句允许同时等待多个 `channel` 操作，从而在多个并发任务之间高效选择一个就绪的任务处理。

**关闭通道**：通道关闭后，仍可读取通道中的剩余数据，这样可以实现通知其他 `goroutine` 数据已处理完毕。

### GMP模型

### **<font color='red'>Go语⾔并发模型</font>**

1、Go 语言的并发模型主要通过 goroutines 和 channels 来实现并发

2、==select 语句==允许在多个通道操作中选择⼀个执⾏。这种⽅式可以有效地处理多个通道的并发操作，避免了阻塞

3、==互斥锁和条件变量：== Go提供了 sync 包，其中包括 Mutex （互斥锁）等同步原语，⽤于在多个goroutines之

间进⾏互斥访问 共享资源。 sync 包还提供了 Cond （条件变量），⽤于在goroutines之间建⽴更复杂的同步。

4、==原⼦操作：== ⽤于在不使⽤锁的情况下进⾏安全的并发操作

**<font color='red'>sync锁底层实现</font>**

`Lock` 方法尝试通过原子操作设置 `state`，如果成功则获取锁。如果失败，则将当前 Goroutine 放入等待队列并

阻塞，直到锁被释放。

`Unlock` 方法通过原子操作修改 `state`，如果有等待的 Goroutine，则唤醒一个等待者。

### **<font color='red'>GMP具体流程</font>**

**程序启动时**：

- 启动时，Go 会根据 `GOMAXPROCS` 环境变量的设置创建 P，默认情况下 P 的数量等于机器的 CPU 核心数。
- M 是动态分配的，程序运行时可以创建多个 M。

**Goroutine 执行**：

- 当你用 `go` 关键字创建一个 goroutine 时，Goroutine 先进入 P 的本地队列。
- P 和 M 绑定后，M 开始从 P 的本地队列中获取 G 来执行。

**Goroutine 阻塞**：

- 如果某个 goroutine 阻塞，例如它在等待某个 I/O 操作完成，那么这个 goroutine 会被挂起，P 可以从本地队列中获取其他的 goroutine 继续执行。

**M 线程不足时的扩展**：

- 如果现有的 M 线程不足以处理所有 goroutine，Go 的调度器会动态创建新的 M。这样，P 就可以继续调度更多的 goroutine 。

**工作窃取（Work Stealing）**：

- 如果一个 P 的本地队列中没有可运行的 goroutine，它会尝试从其他 P 的本地队列中窃取 goroutine，这种机制可以确保负载均衡，防止某些 P 过载，其他 P 空闲。

**Goroutine 完成后**：

- 当一个 M 完成了当前 goroutine 的执行，它会继续从 P 的队列中取下一个 G。如果队列为空且没有其他任务，M 可能会与 P 解绑定并进入空闲状态。

Go语言的 GMP 模型通过将 Goroutine、M（线程）和 P（处理器）进行合理调度，确保了高效的并发执行。该模型具有以下优势：

- **高效并发**：==轻量级的 Goroutine 和高效的调度机制==允许 Go 程序处理大量并发任务。
- **高利用率**：==工作窃取和抢占调度机制==确保了 CPU 资源的高效利用。
- **简化开发**：开发者无需手动管理线程和调度，Go 运行时==自动处理复杂的调度==任务。

### **<font color='red'>GMP 中 p 是如何实现和 陷入内核的m 解绑的？</font>**

在 Go 的 GMP 模型中，当 **M**（内核线程）因系统调用或其他原因陷入阻塞时，**P**（处理器）会与其解绑，并与其他空闲的 **M** 绑定，从而继续执行其他的 Goroutine。

**系统调用阻塞时 P 与 M 的解绑**：

- 当一个 Goroutine 在执行系统调用（例如 I/O 操作）时，负责执行该 Goroutine 的 **M** 可能会陷入内核态并阻塞。
- 当 **M** 被阻塞时，**P** 会立即与 **M** 解绑，以便 **P** 可以被其他非阻塞的 **M** 使用。
- 在这之后，运行时会尝试寻找一个新的 **M** 来绑定这个 **P**，以继续执行其他可运行的 Goroutine。

**阻塞系统调用的处理**：

- 当一个 Goroutine 发起阻塞性系统调用时，当前的 **M** 会陷入阻塞。
- 为了避免调度停滞，Go 运行时会创建一个新的 **M**，这个新的 **M** 会绑定空闲的 **P** 来继续运行其他可执行的 Goroutine。

**M 恢复与 P 的绑定**：

- 当阻塞的 **M**（执行系统调用的线程）完成系统调用并恢复时，它会尝试重新绑定到一个空闲的 **P**。
- 如果没有空闲的 **P**，则这个 **M** 会进入空闲状态，等待将来有空闲的 **P** 可用时再绑定。

### **<font color='red'>在Go中线程和协程和进程的关系到底是怎么样的</font>**

1、进程是资源分配的基本单位，线程是进程的一条执行流程，协程是用户来调度的

2、上下文切换开销：进程大于线程大于协程

3、多进程之间更稳定，线程崩溃一般进程崩溃

4、线程间可以共享内存通信比较方便

### **<font color='red'>如何控制 goroutine 的⽣命周期</font>**

1、使用 `sync.WaitGroup`：可以通过 `Add` 方法设置等待的 goroutine 数量

2、使用通道（Channel）：通过向通道发送信号来控制 goroutine 的退出。

3、使用 `context.Context`：`context.Context` 用于在 goroutine 之间传递取消信号和超时控制。

### **<font color='red'>子协程崩溃了，会影响父协程吗</font>**

**子协程崩溃不会影响父协程**。子协程的 `panic` 只会终止自己，不会传播给其他 goroutine（包括启动它的 goroutine）。

如果希望处理 `panic`，可以使用 `defer` 和 `recover()` 来捕获和恢复 `panic`，防止程序崩溃。

**<font color='red'>为什么协程没有传参也能看见主协程的切片</font>**

在 Go 中，`goroutine` 可以被定义为匿名函数（闭包），这个匿名函数能够访问定义它的外层函数（在这里就是 `main` 函数）的所有变量。闭包捕获的是变量的引用，而不是变量的值。因此，任何在外层作用域中定义的变量都可以直接在 `goroutine` 中使用，而无需显式传递。

```go
for i := 0; i < 5; i++ {
    go func() {
        fmt.Println(i) // 捕获的是同一个 i，并发执行，结果未知
    }()
}
```

### **<font color='red'>什么是互斥锁（mutex）？在什么情况下会⽤到它们？</font>**

```go
package main
import (
	"fmt"
	"sync"
)
// 定义一个全局变量用于计数
var counter int
// 定义一个互斥锁
var mutex sync.Mutex
// 定义一个函数用于增加计数器的值
func increment(wg *sync.WaitGroup) {
	// 函数执行完毕后通知 WaitGroup 计数器减一
	defer wg.Done()
	// 互斥锁加锁，保证在操作共享资源时的互斥性
	mutex.Lock()
	counter++
	// 互斥锁解锁，释放互斥锁，允许其他 goroutine 访问共享资源
	mutex.Unlock()
}
func main() {
	// 创建一个 WaitGroup 用于等待所有 goroutine 执行完毕
	var wg sync.WaitGroup
	// 循环创建 1000 个 goroutine 去执行 increment 函数
	for i := 0; i < 1000; i++ {
		wg.Add(1)      // 每创建一个 goroutine，WaitGroup 计数器加一
		go increment(&wg) // 启动一个新的 goroutine 执行 increment 函数
	}
	// 等待所有 goroutine 执行完毕
	wg.Wait()
	// 打印最终的计数器值
	fmt.Println("Counter:", counter)
}
```

### **<font color='red'>Go什么时候发⽣阻塞？阻塞时调度器会怎么做</font>**

==分类==

等待锁：协程等待其他协程释放锁

通道锁：发送和接收都可能阻塞

系统调用：文件 I/O 操作、网络 I/O 操作

条件变量：协程等待一个条件变量时

==如何处理阻塞==

**将阻塞的 goroutine 标记为不可运行**：调度器会将阻塞的 goroutine 从运行队列中移除，并标记为不可运行状态

**寻找其他可运行的 goroutine**：调度器会检查当前逻辑处理器的本地运行队列和全局运行队列，寻找其他可运行的协程，如果有就会分给当前线程

**创建或唤醒操作系统线程**：如果没有可运行的协程，那么调度器可能让当前线程陷入休眠，如果有新的协程或者某

个阻塞的协程变成可运行状态，调度器会唤醒休眠的线程，或者创建新的线程来处理这些协程

**<font color='red'>Go协程在阻塞的时候其线程有没有阻塞</font>**

1、**阻塞的 goroutine 不会“占用”线程**。Go 运行时会把这个阻塞的 goroutine 从当前线程中移除并挂起。

2、**Go 调度器会将其他就绪的 goroutine 调度到当前线程**，以避免浪费 CPU 资源。

3、如果阻塞的 goroutine 是因为 I/O 等原因导致阻塞，Go 运行时可能会创建新的操作系统线程来处理剩余的任务。

4、**调度器负责管理 goroutine 和线程的关系**，并确保系统不会因为个别 goroutine 的阻塞而浪费计算资源。

**<font color='red'>在IO挂起的时候，是否会影响当前CPU的执行</font>**

在 **I/O 阻塞时**，**goroutine 所在的操作系统线程** 是不会占用 CPU 资源的，**CPU 会被其他任务利用**。Go 运行时会 **调度其他 goroutine** 来执行工作，直到阻塞的 I/O 操作完成，或者会创建新的线程来继续执行剩余任务。因此，**I/O 阻塞**不会直接影响到当前 CPU 的执行，它能够有效地利用 CPU 来执行其他 goroutine。

**<font color='red'>当阻塞事件完成，协程就绪时，系统会通过以下方式知道</font>**

**调度器轮询**：Go 运行时的调度器会周期性地检查所有协程的状态。

**系统调用和事件通知**：在底层，Go 运行时可能会利用操作系统提供的机制来监听阻塞事件的完成。

**被动唤醒**：在某些情况下，协程的阻塞条件可能由其他协程的操作直接唤醒。

### **<font color='red'>go语言是怎么实现网络I/0的</font>**

去看极客兔兔的7天教程

### **<font color='red'>go怎么实现网络连接高并发</font>**

**Goroutine**：goroutine 实现并发处理网络连接，每个客户端请求可以由一个独立的 goroutine 处理，这样可以避

免阻塞主程序或其他连接

**并发模式**：使用 `goroutine` 和 `channel` 实现并发的网络处理

**<font color='red'>如果使用协程来实现网络IO，怎么监听对应文件描述符上可以读写</font>**

1、Go 的 `net` 包本身提供了高效的网络 I/O 操作，内部已经实现了对 I/O 多路复用的支持

2、如果需要更底层的控制，你可以使用 `syscall` 和 `runtime` 包实现 I/O 多路复用

**<font color='red'>什么情况下会因为一个协程阻塞导致其对应的线程阻塞</font>**

**系统调用**： 当一个 goroutine 执行一个阻塞的系统调用（如文件 I/O 操作、网络 I/O 操作等）时，这个系统调用会阻塞整个操作系统线程。在这种情况下，Go 运行时可能会创建一个新的操作系统线程来执行其他 goroutine，以确保程序继续运行。

**死锁**： 如果 goroutine 之间存在死锁，这可能会导致所有相关的 goroutine 阻塞，并可能导致整个程序的所有线程被阻塞。

### **<font color='red'>goroutine什么情况会发⽣内存泄漏？如何避免</font>**

==忘记退出的goroutine :== 如果一个goroutine在等待某些资源或信号（如通道）时没有适当的退出条件，它会永远阻塞，从而导致内存泄漏。

==丢失的goroutine:==如果启动的goroutine与主程序的生命周期脱钩，可能会导致goroutine继续运行并占用资源

==通道泄漏:==当一个通道没有正确关闭，或goroutine在等待数据时未能正常退出，会导致内存泄漏。

解决办法：

1、明确 goroutine 的退出条件，使用上下文（`context`）管理生命周期。

2、适当使用通道并确保通道被正确关闭。

3、避免死锁，通过设计确保资源访问顺序一致。

4、处理通道的发送和接收操作，确保它们成对出现。

5、控制并发数量，避免启动过多的 `goroutine`。

###  **<font color='red'>select的用途？</font>**

select可以理解为是在语言层面实现了和I/O多路复用相似的功能：监听多个描述符的读/写等事件，一旦某个描述

符就绪(一般是读或者写事件发生了)，就能够将发生的事件通知给关心的应用程序去处理该事件。

- select语句只能用于信道的读写操作
- select中的case条件(非阻塞)是并发执行的，select会选择先操作成功的那个case条件去执行，如果多个同时返回，则随机选择一个执行，此时将无法保证执行顺序
- 对于case条件语句中，如果存在信道值为nil的读写操作，则该分支将被忽略，可以理解为从select语句中删除了这个case语句
- 如果有超时条件语句，判断逻辑为如果在这个时间段内一直没有满足条件的case，则执行这个超时case。如果此段时间内出现了可操作的case，则直接执行这个case。一般用超时语句代替了default语句
- 对于空的select{}，会引起死锁
- 对于for中的select{}, 可能会引起cpu占用过高的问题

### **<font color='red'>如何处理 Go 程序中的超时和重试机制？</font>**

1、通过 `context.WithTimeout` 可以为HTTP操作设置超时，当操作超过设定的时间未完成时，操作会被自动取消

2、`select` 语句同时监听 `done` 通道和 `time.After` 通道，哪个先完成就执行哪个分支。在这个例子中，操作耗时 3 秒，但我们设置了 2 秒的超时，因此会打印超时消息

### **<font color='red'>Go的垃圾回收机制</font>**

==GC思想：==

Go中采用最简单的“标记-清除”，“标记有用的对象，清除无用的对象”，采用广度优先搜索算法，从**根集合出发**，进行可达性分析，标记有用对象。

==标记的开始：==根集合是GC在标记过程中**最先检查**的对象，主要包括以下方面：

全局变量：在程序编译期间就能确定，全局变量存在于程序的整个生命周期

执行栈：Go语言中协程是分配在堆上，每个协程都含有自己的执行栈

寄存器：寄存器的值可能表示一个指针，这些指针可能指向另一块内存空间

==并发GC(三色标记法)：==

第一，首先一开始所有的对象都是白色的，从根节点出发，将根节点可达的对象置为灰色。

第二，然后分析灰色节点指向的节点，将灰色节点指向的节点置为灰色节点，分析完一个灰色节点的所有指向后将

灰色该灰色节点置为黑色，表示该节点有用，并且已经扫描完。

第三，重复步骤二，直到没有节点的颜色发生变化，剩下的白色节点就是无用的节点。

==插入屏障：==在并发标记中，一个已经置为黑色的对象A（表示已经扫描分析过了），这时候插入一个**对象C**，并将A

对象指向C对象，在扫描完后C对象仍然是白色的，可能会出现**误清除**。在插入一个对象后（例如插入对象C），将

该对象置成灰色，然后继续扫描，可以防止插入的新元素被误清除。

==删除屏障：==在进行并发标记的过程中，会存在某些对象被删除（记为C），被删除后的对象又被别的已经是黑色的

对象进行连接。这时候，可能出现在全部扫描完后，C对象仍然是白色，会被误清除。加入删除屏障的时候可以避

免该问题的发生，删除屏障即在进行删除操作的时候，将被删除的对象置为灰色，这样在后续的扫描中将，如果该

对象后续有别的对象进行指向则不会被删除，若该对象不存在后续的指向，则将会被清除。

==混合屏障：==**Go的GC使用的是混合屏障，在进行插入和删除的操作的时候将对象置为灰色。**这样就可以实现高并发的垃圾回收，业务和GC可以同时进行。**注意：GC还是会停止业务，在启动屏障的时候会暂停业务一小会**

==GC的触发时机：==

**系统的定时触发**：如果两分钟内没有触发GC，则会每隔两分钟进行触发一次GC。

**用户显示调用**：用户调用runtime.GC方法，进行强制触发

**申请内存时触发**：给对象申请堆空间的时候，可能会触发GC，调用mallocgc的方法。

==优化原则：==从上述的GC的调用时机难以优化GC，GC的优化原则是优化代码的结构，减少在堆上产生垃圾

减少在堆上产生垃圾的方法：

内存池化，内存池，例如channel中采用环形缓存，环形缓存可以一直复用，可以减少垃圾回收

减少逃逸：优化代码，尽可能在栈上分配内存

使用空结构体：空结构体不占用内存空间

==GC的分析工具有：==

- go tool pprof
- go tool trace
- go bulid -gcflags = “-m”
- **GODEBUG=“gctrace=1”**

==总结：==

Go语言可以自动回收堆上分配的内存，并且可以实现并发进行GC，性能较优

Go语言采用的标记方法为三色标记法，并且为了进行并发GC，采用了混合屏障，有效的应对GC过程中的插入和删除操作。

GC有三个触发的时机，分别是，定时触发、用户显示调用触发、申请内存时触发。

GC的优化原则，尽量减少在堆上分配垃圾，内存池化、减少逃逸，使用空结构体。

GC的分析中最常用的命令时GODEBUG=“gctrace=1

**GC** **如何调优**

通过 go tool pprof 和 go tool trace 等工具

- 控制内存分配的速度，限制 goroutine 的数量，从而提高赋值器对 CPU 的利用率
- 减少并复用内存，例如使用 sync.Pool 来复用需要频繁创建临时对象，例如提前分配足够的内存来降低多余的拷⻉
- 需要时，增大 GOGC 的值，降低 GC 的运行频率

### **<font color='red'>介绍⼀下Go中的context包的作⽤</font>**

==一句话：==在 goroutine 之间传递取消信号、截止时间和请求范围内的数据

==1、控制流程和取消操作==

```go
func main() {
	// 创建一个带取消功能的 context
	ctx, cancel := context.WithCancel(context.Background())

	// 启动一个 goroutine，并传递 context
	go func(ctx context.Context) {
		for {
			select {
			case <-ctx.Done():
				fmt.Println("Goroutine 被取消")
				return
			default:
				fmt.Println("Goroutine 正在运行")
				time.Sleep(1 * time.Second)
			}
		}
	}(ctx)

	// 主程序等待 3 秒钟，然后取消 goroutine
	time.Sleep(3 * time.Second)
	cancel()

	// 给 goroutine 一点时间处理取消操作
	time.Sleep(1 * time.Second)
}
```

==2、传递截止时间==

```go
func main() {
	// 设置一个 2 秒的超时时间
	ctx, cancel := context.WithTimeout(context.Background(), 2*time.Second)
	defer cancel()

	select {
	case <-time.After(3 * time.Second):
		fmt.Println("操作成功完成")
	case <-ctx.Done():
		fmt.Println("操作超时：", ctx.Err())
	}
}
```

==3、传递请求范围内的数据==

```go
func main() {
	// 创建一个带有值的 context
	ctx := context.WithValue(context.Background(), "userID", 12345)

	// 启动一个 goroutine 并传递 context
	go func(ctx context.Context) {
		// 获取 context 中的值
		userID := ctx.Value("userID").(int)
		fmt.Println("用户 ID:", userID)
	}(ctx)

	// 等待 goroutine 打印结果
	time.Sleep(1 * time.Second)
}
```

### **<font color='red'>Golang内存管理和操作系统内存管理之间的关系</font>**

==操作系统内存管理==

操作系统负责管理计算机的**物理内存和虚拟内存**，提供内存分配和释放、内存保护、内存映射、分页和交换等功能。其主要职责包括：

**内存分配和释放**：操作系统负责分配和释放进程所需的内存。

**内存保护**：确保一个进程的内存不能被其他进程非法访问。

**虚拟内存**：通过分页和交换机制提供一个比实际物理内存更大的地址空间。

**内存映射**：将文件或设备映射到进程的地址空间。

==Go语言的内存管理==

Go语言提供了一个内存管理子系统，其主要目的是在操作系统内存管理之上提供更高效、更方便的内存分配和垃圾回收功能。

**内存分配**：

- Go语言使用 `runtime` 包提供内存分配功能，常用的分配函数如 `new` 和 `make`。
- `runtime` 包中的内存分配器（malloc）负责从操作系统请求大块内存，并进行细粒度的分配。

**垃圾回收（GC）**：

- Go语言的垃圾回收器负责自动回收不再使用的内存，避免内存泄漏。
- Go使用的是一种并发标记-清除垃圾回收器，它会定期扫描堆内存中的对象，标记不再使用的对象，然后清除这些对象所占用的内存。

**栈和堆**：

- Go语言在栈上分配局部变量，函数调用的上下文等。
- 对于动态分配的内存（如通过 `new` 和 `make` 分配的内存），则在堆上进行分配。
- Go语言的运行时系统会根据需要自动调整栈的大小，以支持深度递归或大量局部变量。

==Go语言内存管理与操作系统内存管理的关系==

**依赖关系**：

- Go语言的内存管理功能建立在操作系统提供的内存管理之上。例如，Go的内存分配器从操作系统请求大块内存，然后进行细粒度的分配。
- Go的垃圾回收器在回收内存时，可能会将空闲的内存块返还给操作系统。

**优化与抽象**：

- Go语言的内存管理针对高性能并发进行了优化，能够更高效地进行内存分配和回收，减少内存碎片和分配开销。
- Go的垃圾回收器旨在减少停顿时间，以提高程序的响应速度和吞吐量。

**协作**：

- 操作系统提供基本的内存管理服务，如内存分配、释放、分页和保护。
- Go运行时系统在此基础上进行更高层次的内存管理，实现垃圾回收和内存复用。

###  **<font color='red'>init函数是什么时候执行的？</font>**

**最终初始化顺序：变量初始化 -> init() -> main()**

### **<font color='red'>反射</font>**

- **Type 和 Value**：`reflect.TypeOf` 返回类型信息，`reflect.ValueOf` 返回值信息。
- **修改值**：需要传递指针并使用 `Elem()` 获取实际值，通过 `CanSet` 检查是否可设置。
- **动态调用方法**：使用 `MethodByName` 查找方法，并使用 `Call` 调用。
- **创建新实例**：使用 `reflect.New` 创建类型的新实例。

### **<font color='red'>怎么避免内存逃逸？</font>**

内存逃逸（Memory Escape）是指变量==从栈上分配转移到堆上==分配的现象。这通常会导致性能下降，因为堆上的内存分配和垃圾回收比栈上的要慢。

- **在方法内把局部变量指针返回**：当函数返回一个局部变量的指针时，由于局部变量会在函数结束后失效，因此编译器无法将该变量分配在栈上，只能分配到堆上。

- **发送指针或带有指针的值到 channel 中。** 在编译时，是没有办法知道哪个 goroutine 会在 channel 上接收数据。所以编译器没法知道变量什么时候才会被释放。

- **在一个切片上存储指针或带指针的值。** 一个典型的例子就是 []*string 。这会导致切片的内容逃逸。尽管其后面的数组可能是在栈上分配的，但其引用的值一定是在堆上。

- **slice 的背后数组被重新分配了，因为 append 时可能会超出其容量( cap )。** slice 初始化的地方在编译时是可以知道的，它最开始会在栈上分配。如果切片背后的存储要基于运行时的数据进行扩充，就会在堆上分配。

- **在 interface 类型上调用方法。** 在 interface 类型上调用方法都是动态调度的 —— 方法的真正实现只能在运行时知道。想像一个 io.Reader 类型的变量 r , 调用 r.Read(b) 会使得 r 的值和切片b 的背后存储都逃逸掉，所以会在堆上分配。

- **闭包捕获外部变量**

  ```go
  func closure() func() {
      x := 10
      return func() {
          fmt.Println(x) // x 逃逸到堆上，因为闭包捕获了 x
      }
  }
  ```

避免方法：

- **尽量使用局部变量**
- **传值而不是传引用**：在函数调用时，尽量传递值类型，而不是指针或引用类型。如果传递的是指针，很可能会导致被传递的变量逃逸到堆上。
- **使用`sync.Pool`进行对象复用**
- **分析逃逸信息：**go build -gcflags="-m" yourfile.go

### 并发编程

**<font color='red'>Golang 中常用的并发模型</font>**

1、简单并发任务可以使用 `goroutines` 和 `channels`

2、共享数据场景适合使用 `sync.Mutex` 和 `sync.WaitGroup`

**sync.Mutex**：互斥锁，保护共享资源，避免多个 `goroutine` 同时访问共享数据时发生冲突。

**sync.RWMutex**：读写锁，允许多个读操作同时进行，但写操作是独占的。

**sync.WaitGroup**：用于等待一组 `goroutine` 完成。可以增加、减少计数和阻塞直到计数为零。

**sync.Once**：用于只执行一次操作的模式（如单例模式）

```go
func main() {
    var wg sync.WaitGroup
    var mu sync.Mutex
    counter := 0

    for i := 0; i < 10; i++ {
        wg.Add(1)
        go func() {
            defer wg.Done()
            mu.Lock()
            counter++
            mu.Unlock()
        }()
    }

    wg.Wait()
    fmt.Println("Counter:", counter)
}
```

**<font color='red'>单例模式</font>**

```go
type Single struct {
	name string
}

var (
	instance *Single
	once     sync.Once
)

func GetSingle() *Single {
	once.Do(func() {
		instance = &Single{name: "hello"}
		fmt.Println("init")
	})
	return instance
}
```

3、控制 `goroutine` 生命周期适合 `context`

```go
func main() {
    // 创建一个带有 2 秒超时的上下文 ctx。2 秒后，ctx 会自动取消。
    ctx, cancel := context.WithTimeout(context.Background(), 2*time.Second)
    defer cancel() // 在 main 函数结束时调用 cancel()，以确保资源释放

    // 启动一个 goroutine，监听 ctx 的 Done 通道，以检测是否取消
    go func(ctx context.Context) {
        select {
        case <-ctx.Done(): // 如果 ctx 被取消或超时
            fmt.Println("Context canceled:", ctx.Err()) // 打印取消原因
        }
    }(ctx)

    // 模拟一些工作，通过 Sleep 暂停 1 秒
    time.Sleep(1 * time.Second)

    // 检查 ctx 是否已经超时或被取消
    if err := ctx.Err(); err != nil {
        fmt.Println("Error:", err) // 如果 ctx 被取消或超时，打印错误信息
    }

    // 获取 ctx 的截止时间
    deadline, ok := ctx.Deadline()
    if ok {
        fmt.Println("Deadline set to:", deadline) // 如果存在截止时间，则打印出来
    } else {
        fmt.Println("No deadline set") // 如果没有截止时间，则打印提示
    }

    // 使用 context.WithValue 在上下文中存储一个键值对
    key := "exampleKey"
    valueCtx := context.WithValue(ctx, key, "exampleValue") // 将键值对绑定到 valueCtx

    // 从 valueCtx 中检索出 key 对应的值
    val := valueCtx.Value(key)
    fmt.Println("Value from context:", val) // 打印出从 context 中取出的值
}
```

4、大规模任务处理适合 `worker pool` 和 `pipeline` 模式

为什么使用 `for...range` 遍历通道

- 通道关闭：for 循环会一直从通道中读取数据，当通道关闭时（例如，使用 close(jobs)），循环会自动结束。
- 通道中没有数据：在通道关闭之前，循环会一直等待新数据的发送（jobs <- value），每当通道接收到一个新值时，循环就会执行一次。
- 当有任务放入 `jobs` 通道时，任意一个 `worker` 都可以接收到它。Go 运行时会选择其中一个空闲的 `worker` 来处理任务。由于通道是无锁的同步数据结构，这种设计的好处是多个 `worker` 能够并行地消费同一通道中的任务，从而达到负载均衡的效果。

```go
func worker(id int, jobs <-chan int, results chan<- int, wg *sync.WaitGroup) {
    defer wg.Done() // 函数结束时，减少 WaitGroup 计数，表示该 worker 完成任务
    for job := range jobs { // 从 jobs 通道中接收任务
        results <- job * 2 // 将处理结果发送到 results 通道（任务值乘以2）
    }
}

func main() {
    // 创建一个带缓冲的 jobs 通道，容量为 5，用于存放任务
    jobs := make(chan int, 5)
    // 创建一个带缓冲的 results 通道，容量为 5，用于存放结果
    results := make(chan int, 5)
    // 定义 WaitGroup，用于等待所有 worker 完成工作
    var wg sync.WaitGroup

    // 启动 3 个 worker goroutine，并将每个 worker 添加到 WaitGroup 中
    for w := 1; w <= 3; w++ {
        wg.Add(1) // 增加 WaitGroup 计数器，表示有一个新的 worker
        go worker(w, jobs, results, &wg) // 启动一个 worker goroutine，传递 id, jobs, results, 和 WaitGroup
    }

    // 发送 5 个任务到 jobs 通道
    for j := 1; j <= 5; j++ {
        jobs <- j // 将任务发送到 jobs 通道
    }
    close(jobs) // 关闭 jobs 通道，通知 workers 没有更多的任务

    // 等待所有的 worker 完成任务
    wg.Wait()
    close(results) // 关闭 results 通道，通知 main 函数没有更多的结果

    // 处理并打印结果
    for res := range results { // 从 results 通道接收并打印处理结果
        fmt.Println("Result:", res)
    }
}
```

```go
func main() {
    // 创建两个无缓冲通道： naturals 和 squares
    naturals := make(chan int)
    squares := make(chan int)

    // 启动第一个 goroutine，将整数发送到 naturals 通道
    go func() {
        for x := 0; x < 5; x++ {
            naturals <- x // 发送 x 到 naturals 通道
        }
        close(naturals) // 关闭 naturals 通道，表示不再发送数据
    }()

    // 启动第二个 goroutine，将从 naturals 接收到的整数平方后发送到 squares 通道
    go func() {
        for x := range naturals { // 从 naturals 通道接收数据，直到通道关闭
            squares <- x * x // 计算平方并发送到 squares 通道
        }
        close(squares) // 关闭 squares 通道，表示不再发送数据
    }()

    // 从 squares 通道接收数据，并打印每个平方结果
    for x := range squares { // 从 squares 通道接收数据，直到通道关闭
        fmt.Println(x) // 打印结果
    }
}
```

**<font color='red'>如何使用 Go 的内置工具进行性能分析和调优（`pprof`，`trace` 等）？</font>**

**<font color='red'>如何减少 Go 程序的内存分配和 GC 压力？</font>**

**<font color='red'>Go 中如何调试内存泄漏？</font>**

### **网络编程与分布式系统**

`《Go Web编程》`看了，深入理解golang如何处理request，标准库中的包如何应对各种`常见的web场景`； 紧接着看下gin，先看极客兔兔的`gin简明教程`，然后上手跟着极客兔兔的博客写web框架，写的过程要去看`gin的官方文档`解决疑惑；        

- Go 在分布式系统中的应用有哪些？如微服务、RPC 通信等。
- Go 中如何实现并发 HTTP 请求处理？

## Go进阶

`for`循环`select`时，如果通道已经关闭会怎么样？如果`select`中的`case`只有一个，又会怎么样？

- `select`中如果任意某个通道有值可读时，它就会被执行，其他被忽略。
  - 其中如果通道被关闭了，那么会返回0，false，然后会一直执行，除非你遇到false就把通道关闭了

- 如果没有`default`字句，`select`将有可能阻塞，直到某个通道有值可以运行，所以`select`里最好有一个`default`，否则将有一直阻塞的风险。

对**已经关闭**的的 `chan` 进行读写，会怎么样？**为什么？**

- 读**已经关闭**的 `chan` 能一直读到东西，但是读到的内容根据通道内`关闭前`是否有元素而不同。
  - 如果 `chan` 关闭前，`buffer` 内有元素**还未读** , 会正确读到 `chan` 内的值，且返回的第二个 bool 值（是否读成功）为 `true`。
  - 如果 `chan` 关闭前，`buffer` 内有元素**已经被读完**，`chan` 内无值，接下来所有接收的值都会非阻塞直接成功，返回 `channel` 元素的**零值**，但是第二个 `bool` 值一直为 `false`。
- 写**已经关闭**的 `chan` 会 `panic`

能说说uintptr和unsafe.Pointer的区别吗？

- unsafe.Pointer只是单纯的通用指针类型，用于转换不同类型指针，它不可以参与指针运算；
- 而uintptr是用于指针运算的，GC 不把 uintptr 当指针，也就是说 uintptr 无法持有对象， uintptr 类型的目标会被回收；
- unsafe.Pointer 可以和 普通指针 进行相互转换；
- unsafe.Pointer 可以和 uintptr 进行相互转换。

```go
func main() {
 var w *W = new(W)
 //这时w的变量打印出来都是默认值0，0
 fmt.Println(w.b,w.c)

 //现在我们通过指针运算给b变量赋值为10
 b := unsafe.Pointer(uintptr(unsafe.Pointer(w)) + unsafe.Offsetof(w.b))
 *((*int)(b)) = 10
 //此时结果就变成了10，0
 fmt.Println(w.b,w.c)
}
```

**协程如何调度？**

协程：协程拥有自己的寄存器上下文和栈。协程调度切换时，将寄存器上下文和栈保存到其他地方，在切回来的时候，恢 复先前保存的寄存器上下文和栈。

进入上一次离开时所处逻辑流的位置。 线程和进程的 操作是由程序触发系统接口，最后的执行者是系统;协程的操作执行者则是用户自身程序，goroutine也是协程

GMP模型：

- M代表内核级线程，goroutine就是跑在M之上的，M是一个很大的结构，里面维护小对象内存cache(mcache)、当前执行的goroutine、随机数发生器等等非常多的信息
- G，代表一个goroutine，它有自己的栈，正在等待的channel等等，用于调度
- P，它的主要用途就是用来执行goroutine的，所以它也维护了一个goroutine队列，里面存储了所有需要它来执行的goroutine 
- Sched:代表调度器，它维护有存储M和G的队列以及调度器的一些状态信息等。

### **<font color='red'>说一下 GMP 模型？GMP 模型对于内存占用会有影响吗？为什么要设计 GMP 模型？</font>**

### 如果M数量一定，P的数量过大和过小有啥区别



