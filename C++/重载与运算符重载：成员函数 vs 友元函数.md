---
date: 2026-11-10
created: 2026-11-10
---

# 重载与运算符重载：成员函数 vs 友元函数

创建日期：2026-11-10

## 0. 一句话概括

**重载**是"同一个名字、不同的参数列表"，由编译器在**编译期**决定调用哪一个；**运算符重载**只是把 `+`、`<<` 这些符号写成函数。写成**成员函数**时左操作数隐含是 `*this`（左操作数不能做隐式转换）；写成**友元函数**时两个操作数都是普通参数（左右都能隐式转换，也能让流对象待在左边）。

## 1. 什么是重载

### 1.1 函数重载：同一个名字，不同的参数列表

```cpp
void print(int x);          // (1)
void print(double x);       // (2)
void print(const char* s);  // (3)
void print(int a, int b);   // (4)
```

四个 `print` 名字一样、参数列表不同，编译器根据**实参的类型和个数**在编译期挑一个，这个过程叫**重载决议**（overload resolution）。它是**编译期多态**：运行时不查表、没有额外开销。

### 1.2 三条容易记错的规则

| 规则 | 能不能重载 | 说明 |
| --- | --- | --- |
| 参数**个数/类型/顺序**不同 | ✅ | 这是重载的**唯一**依据 |
| **只有返回类型**不同 | ❌ | 调用 `f(x);` 时编译器不看返回值，无法区分 |
| 顶层 `const` 修饰**值参数** | ❌ | `void f(int)` 与 `void f(const int)` 是**同一个**函数 |
| 指针 / 引用参数上的 `const` | ✅ | `void f(int&)` 与 `void f(const int&)` 是两个函数 |
| 默认实参不同 | ⚠️ | 可能造成**二义性**：`f(1)` 同时匹配两个版本时报错 |

### 1.3 别把"重载"和"覆盖、隐藏"搞混

| 名称 | 英文 | 发生的位置 | 触发条件 |
| --- | --- | --- | --- |
| **重载** | overload | **同一个作用域**（同一个类或同一个命名空间） | 同名、**参数列表不同** |
| **覆盖** | override | 派生类对基类 | 基类函数是 `virtual`，派生类函数**签名完全相同**（C++11 起可写 `override` 让编译器检查） |
| **隐藏** | hide | 派生类对基类 | 派生类写了同名函数，参数不同或基类非虚 —— 基类的版本被"挡住"，除非用 `using` 引入 |

## 2. 运算符重载是什么

### 2.1 本质：一种"名字很特殊的函数"

`a + b` 在编译器眼里等价于 `operator+(a, b)`；`a += b` 等价于 `operator+=(a, b)`。所谓"重载运算符"，就是**给这个特殊名字定义一个函数**，函数体里写你希望 `+` 做的事。

### 2.2 四条硬性限制

- **至少一个操作数必须是类或枚举类型**：不能改变 `int + int` 的含义；
- **不能改变**优先级、结合性、操作数个数；
- **不能发明新运算符**（没有 `**`、`<=>` 之外的"自定义符号"）；
- **不能重载**：`::`（作用域）、`.`（成员访问）、`.*`、`?:`、`sizeof`、`alignof`、`typeid`、`#`/`##`（预处理，不归编译器管）。

### 2.3 有四个必须写成成员函数

`=`、`[]`、`()`、`->` 这四个**只能**定义为类的非静态成员函数。原因是语言规定它们必须绑定在对象上，这样编译器才能保证 `a[i] = v`、`p->x` 这类写法的语义。其余运算符既可以写成成员，也可以写成非成员（友元或普通自由函数）。

## 3. 写成成员函数

### 3.1 形式

```cpp
class T {
public:
    T operator+(const T& rhs) const;   // 双目：rhs 是右操作数，*this 是左操作数
    T& operator+=(const T& rhs);       // 单目自赋值类：通常返回 *this 的引用
};
```

调用 `a + b` 时，编译器把它看成 `a.operator+(b)` —— **左操作数必须是 `T` 类型的对象**（或能绑定到 `*this` 的东西）。

### 3.2 例子

```cpp
class Complex {
public:
    Complex(double r = 0.0, double i = 0.0) : re(r), im(i) {}

    Complex operator+(const Complex& rhs) const {
        return Complex(re + rhs.re, im + rhs.im);
    }

    Complex& operator+=(const Complex& rhs) {
        re += rhs.re;
        im += rhs.im;
        return *this;          // 返回引用，才能 a += b += c
    }

private:
    double re, im;
};
```

### 3.3 局限：左操作数不能做隐式转换

`Complex` 的构造函数不是 `explicit`，所以 `double` 能隐式转成 `Complex`。于是：

```cpp
Complex a(1, 2);
Complex c1 = a + 2.5;   // ✅ 2.5 隐式转成 Complex，调用 a.operator+(Complex(2.5))
Complex c2 = 2.5 + a;   // ❌ 编译错误：2.5 不是 Complex，找不到 operator+(double, Complex)
```

**原因**：成员函数的"隐含对象参数"（`*this`）**不参与隐式类型转换**。`2.5 + a` 要求左边是 `Complex`，而 `2.5` 是 `double`，编译器不会为它找 `Complex` 的转换构造函数。

这就是为什么双目运算符常常还要**再写一个非成员版本**（下节）。

## 4. 写成友元函数

### 4.1 形式

```cpp
class T {
    friend T operator+(double k, const T& rhs);   // 不是成员，但能访问 private
};
```

友元函数**不是类的成员**：它没有 `this`，两个操作数都写在参数表里，因此**左右操作数都能做隐式转换**。

### 4.2 例子

```cpp
class Complex {
public:
    Complex(double r = 0.0, double i = 0.0) : re(r), im(i) {}

    friend Complex operator+(double k, const Complex& c) {
        return Complex(k + c.re, c.im);      // 直接访问 private 成员
    }

private:
    double re, im;
};
```

现在 `2.5 + a` 就能编译了：编译器找到 `operator+(double, const Complex&)`，把 `2.5` 当第一个参数 ✓

### 4.3 为什么 `<<`、`>>` 必须用非成员

```cpp
std::cout << a;   // 左操作是 std::ostream，右操作才是 Complex
```

如果写成成员函数，就会变成 `a.operator<<(std::cout)`，调用得上却要写成 `a << std::cout`，与本意完全相反。**流的类型在最左边，我们无权改 `std::ostream` 的类定义**，所以只能写非成员：

```cpp
friend std::ostream& operator<<(std::ostream& os, const Complex& c) {
    os << c.re << (c.im >= 0 ? "+" : "") << c.im << "i";
    return os;          // 返回 os 的引用，才能 cout << a << b << endl
}
```

注意返回类型是 **`std::ostream&`**（不是 `void`、也不是值）：只有返回引用，`cout << a << b` 才能接着往右串。

## 5. 一个完整的小例子

### 5.1 代码

```cpp
#include <iostream>

class Complex {
public:
    Complex(double r = 0.0, double i = 0.0) : re(r), im(i) {}

    // 成员函数：Complex + Complex
    Complex operator+(const Complex& rhs) const {
        return Complex(re + rhs.re, im + rhs.im);
    }

    // 成员函数：Complex += Complex（返回引用）
    Complex& operator+=(const Complex& rhs) {
        re += rhs.re;
        im += rhs.im;
        return *this;
    }

    // 友元：double + Complex（左操作数要能隐式转换）
    friend Complex operator+(double k, const Complex& c) {
        return Complex(k + c.re, c.im);
    }

    // 友元：输出流（ostream 必须在左边）
    friend std::ostream& operator<<(std::ostream& os, const Complex& c) {
        os << c.re << (c.im >= 0 ? "+" : "") << c.im << "i";
        return os;
    }

private:
    double re, im;
};

int main() {
    Complex a(1, 2), b(3, 4);

    std::cout << a + b << '\n';     // 成员版：4+6i
    std::cout << 2.5 + a << '\n';   // 友元版：3.5+2i

    a += b;                         // 成员版：a 变成 (4, 6)
    std::cout << a << '\n';         // 友元版：4+6i
}
```

### 5.2 运行结果

```text
4+6i
3.5+2i
4+6i
```

### 5.3 逐句对照

| 代码 | 编译器实际调用 | 用的是谁 |
| --- | --- | --- |
| `a + b` | `a.operator+(b)` | 成员函数 |
| `2.5 + a` | `operator+(2.5, a)` | 友元函数 |
| `a += b` | `a.operator+=(b)` | 成员函数 |
| `std::cout << a` | `operator<<(std::cout, a)` | 友元函数 |

## 6. 怎么选：成员还是友元

| 场景 | 推荐 | 原因 |
| --- | --- | --- |
| `=` `[]` `()` `->` | **必须成员** | 语言规定 |
| `+=` `-=` `*=` `++` `--` 等修改自身的 | **成员** | 语义上"改自己"，返回 `*this` 最自然 |
| 左操作数可能是基本类型的（`2 + obj`） | **友元** | 成员版无法转换左操作数 |
| `<<` `>>` 流输入输出 | **友元** | 流对象在左边 |
| 需要对称性的双目运算（`a+b` 与 `b+a`） | 成员 + 友元各一个，或都用友元 | 避免一边能编译一边不能 |

> 补充：友元通常定义为**非成员的非友元**也行——只要它只用到类的 public 接口（比如 `getRe()`），就不必是友元。**能用公开接口就别用友元**，减少耦合。

## 7. 几个易错点

1. **`operator=` 要自己写吗**：如果类里没有指针/资源，编译器默认生成的逐成员拷贝就够用；一旦管理了堆内存，必须自己写（并配套写拷贝构造、析构 —— Rule of Three），否则会浅拷贝导致双重释放。
2. **`operator+` 别返回引用**：它产生的是**新对象**，返回局部对象的引用就是悬空引用 ✗；只有 `+=`、`=`、`<<` 这类"返回已有对象"的才返回引用。
3. **前置 `++` 与后置 `++`**：前置写 `T& operator++()`，后置写 `T operator++(int)`（那个 `int` 是哑元，只用来区分）。后置要先保存旧值再自增，再返回旧值的副本。
4. **不要重载 `&&`、`||`、`,`**：重载后**短路求值会消失**，而且逗号的求值顺序也不再保证，容易埋坑。
5. **`operator bool` 要 `explicit`**：否则对象会被隐式转成 `int`，出现 `if (a < b)` 被解释成整数比较之类的怪事。

## 8. 记忆框架

| 层次 | 要点 |
| --- | --- |
| **重载是什么** | 同一名字、不同参数列表，**编译期**选版本（编译期多态） |
| **只靠返回值不行** | 返回类型不参与重载决议 |
| **运算符重载是什么** | 给 `operator+` 这样的特殊名字写函数；至少一个操作数是自定义类型 |
| **成员写法** | `a + b` → `a.operator+(b)`，左操作数是 `*this`，**左操作数不能隐式转换** |
| **友元写法** | 两个操作数都是参数，**左右都能隐式转换**；`<<`/`>>` 必须这么写 |
| **返回值** | 产生新对象用值；`+=`/`=`/`<<` 返回引用 |
| **选择口诀** | 改自身 → 成员；要转换左操作数、要流 → 友元 |
