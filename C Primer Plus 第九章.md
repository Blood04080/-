# 2025/11/13 函数

## 函数

### 函数的概述
函数是用于执行特定功能的独立代码单元。使用函数可以：
- 避免重复编写代码
- 提高代码的可读性和可维护性
- 促进模块化编程
- 便于团队协作和后续修改

### 创建并使用简单的函数
在C语言中，使用自定义函数需要先声明或定义。有两种常见方式：

**方式一：先声明后定义**
```C
// 函数声明（函数原型）
int calculateSum(int a, int b);

int main(void) {
    int result = calculateSum(5, 3); // 函数调用
    return 0;
}

// 函数定义
int calculateSum(int a, int b) {
    return a + b;
}
```

**方式二：直接定义**
```C
// 函数定义
int calculateSum(int a, int b) {
    return a + b;
}

int main(void) {
    int result = calculateSum(5, 3); // 函数调用
    return 0;
}
```

**三个关键概念：**
1. **函数声明**：向编译器说明函数的接口（返回值类型、函数名、参数列表）
2. **函数调用**：在程序中使用函数，传递实际参数
3. **函数定义**：具体实现函数的功能

**重要说明：**
- 函数原型 = 返回值类型 + 函数名 + 参数列表
- 在主函数调用之前，编译器必须知道函数的原型或定义
- 函数原型可以放在主函数之前或变量声明区域

### 函数的参数
**函数原型示例：**
```C
int calculateSum(int a, int b);
```

**关键概念：**
- **形式参数**：函数定义中的参数（如 `a`, `b`）
  - 只在函数内部有效
  - 函数调用时在栈上分配内存
  - 与外部同名变量不会冲突
- **实际参数**：调用函数时传递的具体值

### 栈的概念
栈是一种后进先出（LIFO）的数据结构，在函数调用中用于：
- 存储局部变量
- 保存函数返回地址
- 传递参数
- 保存寄存器状态

## 递归

### 递归基本原理
递归是指函数直接或间接调用自身的过程。

**递归三要素：**
1. **递归出口**：终止条件，防止无限递归
2. **递归调用**：函数调用自身
3. **问题简化**：每次递归调用都使问题规模减小

**递归示例：计算阶乘**
```C
int factorial(int n) {
    if (n <= 1) return 1;           // 递归出口
    else return n * factorial(n - 1); // 递归调用，问题规模从n减小到n-1
}
```

### 尾递归
在函数末尾调用自身的特殊递归形式，可以被编译器优化为迭代，减少栈空间使用。

**尾递归示例：**
```C
int factorial_tail(int n, int accumulator) {
    if (n <= 1) return accumulator;
    return factorial_tail(n - 1, n * accumulator);
}
```

## 使用多源代码文件
对于大型项目，可以将相关函数组织到不同的源文件中：

**math_functions.h（头文件）**
```C
#ifndef MATH_FUNCTIONS_H
#define MATH_FUNCTIONS_H

int calculateSum(int a, int b);
int calculateProduct(int a, int b);

#endif
```

**math_functions.c（实现文件）**
```C
#include "math_functions.h"

int calculateSum(int a, int b) {
    return a + b;
}

int calculateProduct(int a, int b) {
    return a * b;
}
```

**main.c（主文件）**
```C
#include <stdio.h>
#include "math_functions.h"  // 使用双引号包含自定义头文件

int main() {
    printf("Sum: %d\n", calculateSum(5, 3));
    return 0;
}
```

**编译命令：**
```bash
gcc main.c math_functions.c -o program
```

## 指针

### 查找地址：&运算符
**核心概念：**
- 如果被调函数需要修改主调函数中的变量，必须通过传递变量的地址
- `&` 是取地址运算符
- 如果 `pooh` 是变量，那么 `&pooh` 就是它的地址
- 地址是常量，在变量生命周期内不变

### 简单的指针

#### 简介
指针是存储内存地址的变量。

```C
int pooh = 10;
int *ptr = &pooh;  // ptr指向pooh
```

#### * 间接运算符（解引用运算符）
```C
int val = *ptr;  // 获取ptr指向的变量的值
```

#### 声明指针
```C
int *ptr;        // 指向整型的指针
float *fptr;     // 指向浮点数的指针
char *cptr;      // 指向字符的指针
```

**重要：指针初始化**
```C
int *ptr = NULL;  // 初始化为空指针，避免野指针
int x = 10;
ptr = &x;         // 指向有效变量
```

#### 使用指针在函数间通信
```C
void swap(int *a, int *b) {
    int temp = *a;
    *a = *b;
    *b = temp;
}

int main() {
    int x = 5, y = 10;
    swap(&x, &y);  // 传递地址
    return 0;
}
```

**指针解引用规则：**
- 在赋值语句左侧：`*ptr = value` → 修改指向的变量
- 在赋值语句右侧：`value = *ptr` → 获取指向变量的值

---
