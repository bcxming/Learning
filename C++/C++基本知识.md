### C++的基本语法

#### 变量与数据类型

C++支持多种数据类型，常见的数据类型包括：

- **整数类型**：`int`、`short`、`long`、`long long`等。
- **浮点类型**：`float`、`double`、`long double`。
- **字符类型**：`char`，用于存储单个字符。
- **布尔类型**：`bool`，用于存储`true`或`false`。

定义变量时，需要指定变量的类型：

```cpp
int age = 25;  // 整数类型
float height = 175.5;  // 浮点类型
char initial = 'A';  // 字符类型
bool isStudent = true;  // 布尔类型
```

#### 基本操作符

C++中有多种操作符，用于执行各种基本操作。常见的操作符包括：

- **算术操作符**：`+`（加）、`-`（减）、`*`（乘）、`/`（除）、`%`（取余）。
- **比较操作符**：`==`（等于）、`!=`（不等于）、`<`（小于）、`>`（大于）、`<=`（小于等于）、`>=`（大于等于）。
- **逻辑操作符**：`&&`（与）、`||`（或）、`!`（非）。

```cpp
int a = 10, b = 20;
int sum = a + b;  // 加法
bool isEqual = (a == b);  // 比较两个数是否相等
bool isGreater = (a > b) && (b > 5);  // 逻辑与操作
```

### 控制流语句

控制流语句决定了程序的执行流程。常见的控制流语句包括条件语句和循环语句。

#### 条件语句

条件语句用于根据条件的真假来决定是否执行某段代码。C++中最常用的条件语句是`if`语句和`switch`语句。

```cpp
int score = 85;

if (score >= 90) {
    std::cout << "Grade: A" << std::endl;
} else if (score >= 80) {
    std::cout << "Grade: B" << std::endl;
} else {
    std::cout << "Grade: C" << std::endl;
}
```

在这个例子中，根据`score`的值，程序会输出不同的等级。

#### 循环语句

循环语句用于重复执行某段代码。C++中的常见循环语句包括`for`循环、`while`循环和`do-while`循环。

```cpp
// for循环示例
for (int i = 0; i < 5; ++i) {
    std::cout << "i: " << i << std::endl;
}

// while循环示例
int j = 0;
while (j < 5) {
    std::cout << "j: " << j << std::endl;
    ++j;
}

// do-while循环示例
int k = 0;
do {
    std::cout << "k: " << k << std::endl;
    ++k;
} while (k < 5);
```

在这些示例中，`for`循环、`while`循环和`do-while`循环都用于重复输出变量的值。

### 函数

函数是C++中的基本代码组织单位。它们使得代码更具模块化和可重用性。函数可以接收参数，并返回一个值。

```cpp
int add(int x, int y) {
    return x + y;
}

int main() {
    int result = add(3, 4);
    std::cout << "Sum: " << result << std::endl;
    return 0;
}
```

在这个例子中，定义了一个名为`add`的函数，它接收两个整数参数，并返回它们的和。在`main`函数中，调用`add`函数并输出结果。

### 数组与字符串

#### 数组

数组是用于存储多个相同类型数据的容器。数组的大小在定义时确定，不能动态改变。

```cpp
int numbers[5] = {1, 2, 3, 4, 5};

for (int i = 0; i < 5; ++i) {
    std::cout << "numbers[" << i << "]: " << numbers[i] << std::endl;
}
```

#### 字符串

C++中的字符串可以使用字符数组，也可以使用标准库中的`std::string`类。

```cpp
char str[] = "Hello";
std::string greeting = "Hello, World!";

std::cout << str << std::endl;
std::cout << greeting << std::endl;
```

`std::string`提供了许多操作字符串的函数，使得处理文本更加方便。

### 指针与引用

#### 指针

指针是C++中的一个非常重要的概念，它用于存储变量的内存地址。通过指针，程序可以直接访问和操作内存。

```cpp
int value = 42;
int* ptr = &value;  // 指针ptr指向value的地址

std::cout << "Value: " << value << std::endl;
std::cout << "Pointer: " << ptr << std::endl;
std::cout << "Dereferenced Pointer: " << *ptr << std::endl;  // 通过指针访问值
```

#### 引用

引用是指向某个变量的别名，一旦引用被初始化后，就不能改变其指向的对象。引用的语法更加简洁，且使用起来更加安全。

```cpp
int a = 10;
int& ref = a;  // ref是a的引用

std::cout << "a: " << a << std::endl;
std::cout << "ref: " << ref << std::endl;

ref = 20;  // 通过引用修改a的值
std::cout << "a after modification: " << a << std::endl;
```

### 面向对象编程

C++是一门支持面向对象编程（OOP）的语言。OOP的基本概念包括类、对象、继承和多态。

#### 类和对象

类是用户定义的数据类型，它封装了数据和操作这些数据的函数。对象是类的实例。

```cpp
class Dog {
public:
    std::string name;
    int age;

    void bark() {
        std::cout << name << " says: Woof!" << std::endl;
    }
};

int main() {
    Dog myDog;
    myDog.name = "Buddy";
    myDog.age = 3;

    myDog.bark();  // 输出“Buddy says: Woof!”
    return 0;
}
```

#### 继承和多态

继承允许一个类从另一个类派生，继承父类的属性和方法。多态允许使用父类的指针或引用调用子类的重写方法。

```cpp
class Animal {
public:
    virtual void speak() {
        std::cout << "Animal sound" << std::endl;
    }
};

class Cat : public Animal {
public:
    void speak() override {
        std::cout << "Meow" << std::endl;
    }
};

int main() {
    Animal* myAnimal = new Cat();
    myAnimal->speak();  // 输出“Meow”，多态的表现
    delete myAnimal;
    return 0;
}
```

在这个例子中，`Cat`类继承了`Animal`类，并重写了`speak`方法。通过父类指针调用子类方法展示了多态的特性。
