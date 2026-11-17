# C 语言术语表

> **用途**：本仓库所有 C 卡片统一使用这里的译法。看到一个术语，先来这里对齐含义，
> 再去读卡片。卡片正文首次出现术语时 MUST 写成「中文（English）」形式，其后可只用中文或只用英文。
>
> **标准基准**：`C17`。本表只收录**标准 C** 的术语；GNU 扩展、编译器内建等见
> [B16 标准版本与编译器扩展](./B16_standards/标准版本与编译器扩展.md)；
> 与 C++ 的对照见 `CPP/GLOSSARY_CPP.md`（C++ 是 C 的超集，那份表 MUST 完整包含本表全部术语）。
>
> **读法**：`中文（English）` —— 一句话说清它是什么、最容易踩的坑在哪。

---

## 一、基本类型与限定符

| 术语 | English | 说明 |
|------|---------|------|
| 整型 | integer type | `int`、`short`、`long`、`long long` 及其 `signed`/`unsigned` 变体。宽度由实现决定，`sizeof` 才是准的 |
| 无符号整型 | unsigned integer type | 取值范围从 0 开始。**有符号转无符号是隐式转换**，负数会变成巨大的正数 |
| 整型提升 | integer promotion | 小于 `int` 的类型参与运算时先提升为 `int` 或 `unsigned int` |
| 浮点型 | floating-point type | `float`/`double`/`long double`。二进制无法精确表示 0.1，`0.1+0.2 != 0.3` |
| 布尔型 | boolean type | C17 里是 `_Bool`；C23 起 `bool` 成为关键字。**任何非零指针都转成 1** |
| 字符型 | character type | `char` 存的是**字节**不是字符，一个 `char` 装不下一个 UTF-8 汉字 |
| 无符号字符 | unsigned char | 唯一能可靠表示任意字节的类型，`unsigned char` 与 `uint8_t` 常同义 |
| 空类型 | void type | `void` 不能有对象；`void *` 是通用对象指针 |
| 尺寸类型 | `size_t` | 无符号整型，** sizeof 的返回值类型**，用来表示"大小"和"下标" |
| 指针差类型 | `ptrdiff_t` | 有符号整型，两指针相减的结果类型 |
| 常量限定 | `const` qualification | `const` 修饰的是**对象**不可通过该名字修改，不是"编译期常量" |
| 易变限定 | `volatile` qualification | 告诉编译器"别优化它"，用于内存映射寄存器。**挡不住并发问题** |
| restrict 限定 | `restrict` qualification | 承诺该指针访问的对象不被别的途径访问，是优化提示，不是安全保证 |
| 存储类说明符 | storage-class specifier | `typedef`、`extern`、`static`、`auto`、`register`、`_Thread_local` |
| 类型别名 | `typedef` | 给已有类型起别名，**不创建新类型** |
| 枚举类型 | enumerated type | `enum`，底层类型由实现决定（通常 `int`）。**枚举变量可以取枚举列表外的值** |
| 不透明指针 | opaque pointer | 头文件里只写 `typedef struct Foo Foo;`，隐藏结构体定义 |

---

## 二、声明、定义与作用域

| 术语 | English | 说明 |
|------|---------|------|
| 声明 | declaration | 告诉编译器"有这么个东西"，如 `extern int x;` |
| 定义 | definition | 真正分配存储或给出实现，如 `int x = 0;`、`void f(void) {}` |
| 函数原型 | function prototype | 带完整参数类型的声明 `int f(int, char *)` |
| 作用域 | scope | 名字可见的范围：块作用域、函数原型作用域、文件作用域、函数作用域 |
| 存储期 | storage duration | 对象的存活期：自动、静态、线程、分配 |
| 链接属性 | linkage | 名字在哪个文件可见：外部链接、内部链接、无链接 |
| 遮蔽 | shadowing | 内层作用域的同名变量遮住外层的 |
| 初始化 | initialization | 声明时给初值。**未初始化的局部变量是未定义值，不是 0** |
| 类型转换 | conversion | 隐式（编译器自动）与显式（强制转换） |
| 类型别名声明 | typedef declaration | 见「类型别名」 |
| 不完整类型 | incomplete type | 知道大小但不知道布局，如 `struct Node;` |

---

## 三、运算符与表达式

| 术语 | English | 说明 |
|------|---------|------|
| 表达式 | expression | 由运算符和操作数构成，**求值产生一个值，也可能产生副作用** |
| 副作用 | side effect | 对程序状态的影响，如赋值、修改对象 |
| 序列点 | sequence point | 求值顺序的分界点。**C11 起官方术语是 "sequenced before"** |
| 短路求值 | short-circuit evaluation | `&&` 与 `||` 的右操作数可能不执行 |
| 复合赋值 | compound assignment | `a += b` 形式，**结果类型与 `a` 相同**，与 `a = a + b` 可能不同 |
| 整除 | integer division | 整数相除截断取整，向零舍入，不是向下取整 |
| 取余 | remainder | `a % b` 的符号跟随被除数 |
| 位运算 | bitwise operation | `&`、`|`、`^`、`~`、`<<`、`>>`。移位位数 ≥ 类型宽度是未定义行为 |
| 条件运算符 | conditional operator | `a ? b : c`，唯一的三目运算符 |
| 逗号运算符 | comma operator | 左边先求值，值是右边的。优先级最低 |
| sizeof | sizeof operator | 返回**字节数**；`sizeof` 数组得整个数组大小，`sizeof` 指针得指针大小 |
| 复合字面量 | compound literal | `(struct Point){1, 2}`，C99 引入，可在块作用域构造临时对象 |
| 左值 | lvalue | 能放在赋值号左边的表达式，本质是**有地址的对象** |
| 修改型左值 | modifiable lvalue | 左值且非数组、非 const、非结构体含 const 成员 |

---

## 四、指针与内存

| 术语 | English | 说明 |
|------|---------|------|
| 地址 | address | 对象在内存中的位置 |
| 指针 | pointer | 存放地址的变量，**类型决定它指向什么、以及算术步长** |
| 取地址 | address-of | `&x` |
| 解引用 | dereference | `*p` |
| 空指针 | null pointer | `NULL`，不指向任何对象。**解引用空指针是未定义行为** |
| 野指针 | wild pointer | 未初始化或已失效的指针值 |
| 悬垂指针 | dangling pointer | 指向已释放内存或已出作用域对象的指针 |
| 指针算术 | pointer arithmetic | `p + 1` 前进**一个元素**，不是一字节；只能在同一数组内运算 |
| 数组退化 | array-to-pointer decay | 数组在多数表达式中隐式转成指向首元素的指针；`sizeof`、`&`、字符串字面量初始化除外 |
| void 指针 | `void *` | 类型擦除的通用指针，转回具体类型需要强制转换 |
| 指针的指针 | pointer to pointer | `char **`，修改指针本身时需要它 |
| 对齐 | alignment | 类型要求的起始地址约束，如 `int` 通常要求 4 字节对齐 |
| 填充 | padding | 结构体成员之间为满足对齐而插入的空字节 |
| 内存布局 | memory layout | 成员顺序决定结构体在内存里长什么样 |
| 字节序 | endianness | 多字节整数的存储顺序：大端（big-endian）/ 小端（little-endian） |

---

## 五、数组与字符串

| 术语 | English | 说明 |
|------|---------|------|
| 数组 | array | 连续存放的同类型元素，**长度在类型里**，不能像 Python list 那样 append |
| 多维数组 | multidimensional array | 语法上是"数组的数组"，内存里仍是**一维连续块** |
| 字符串 | string | C 里没有字符串类型，只有"**以 `\0` 结尾的字符序列**" |
| NUL 终止符 | null terminator | `'\0'`，字符串结束的标志，**不计入 `strlen` 的结果** |
| 字符串字面量 | string literal | 类型是 `char[N]`（**不是** `char *`），**修改它是未定义行为** |
| 字符数组与字符指针 | char array vs char pointer | `char a[]="x"` 可改，`char *p="x"` 指向只读区 |

---

## 六、结构体与联合体

| 术语 | English | 说明 |
|------|---------|------|
| 结构体 | structure (`struct`) | 把一组相关字段聚合成一种类型。**赋值可以，比较不行** |
| 联合体 | union (`union`) | 所有成员共用一块内存，**同一时刻只有一个成员有效** |
| 位域 | bit-field | 指定成员占几个二进制位，省空间但**布局不可移植** |
| 匿名结构体 | anonymous struct | 无名成员类型，可直接用成员名访问，C11 起标准 |
| 柔性数组 | flexible array member | 结构体末尾的 `T arr[]`，长度由外部决定，**必须用动态分配** |
| 自引用结构体 | self-referential struct | 成员含自身类型的指针，构成链表/树的节点 |
| 类型双关 | type punning | 用 union 或指针强转读写同一块内存的不同类型 |

---

## 七、函数

| 术语 | English | 说明 |
|------|---------|------|
| 函数定义 | function definition | 给出实现 |
| 值传递 | pass by value | **C 只有值传递**。"传指针"传的是地址的副本 |
| 输出参数 | output parameter | `int *out` 这种约定，靠指针回写结果，**调用方必须初始化** |
| 可变参数 | variadic function | `...` + `va_list`/`va_start`/`va_arg`/`va_end` |
| 递归 | recursion | 函数调用自身，需有终止条件，注意栈开销 |
| 静态函数 | static function | 文件作用域的 `static`，**符号不对外可见** |
| 内联函数 | inline function | 建议编译器展开；`static inline` 是内部链接 |
| 函数指针 | function pointer | 把函数当值传。**声明时要写全括号**：`int (*fp)(int)` |
| 回调 | callback | 由调用方传入、供被调方调用的函数指针 |
| 闭包 | closure | **C 没有闭包**。等价物是 `static` 局部变量或 context 结构体 + 函数指针 |
| 上下文结构体 | context struct | 把回调需要的状态打包成结构体一起传，显式化的闭包 |

---

## 八、预处理器与编译

| 术语 | English | 说明 |
|------|---------|------|
| 翻译单元 | translation unit | 一个 `.c` 文件经预处理后的产物 |
| 预处理 | preprocessing | 展开宏、条件编译，**它只是文本处理器，不是编译器** |
| 对象宏 | object-like macro | `#define N 10`，无参数 |
| 函数宏 | function-like macro | `#define MAX(a,b) ...`，**参数与整个替换体都要加括号** |
| 包含守卫 | include guard | `#ifndef`/`#define`/`#endif` 防重复包含 |
| 条件编译 | conditional compilation | `#if`/`#ifdef`/`#else`，用于平台差异 |
| 内建宏 | predefined macro | `__FILE__`、`__LINE__`、`__DATE__`、`__STDC__`、`__STDC_VERSION__` |
| 静态断言 | static assertion | `_Static_assert(cond, "msg")`，编译期检查常量条件 |
| 链接 | linking | 把多个目标文件合成可执行文件。**声明给了承诺，定义给兑现** |
| ABI | application binary interface | 二进制接口约定。**MinGW-w64 与 MSVC 的 ABI 不兼容，产物无法互链** |

---

## 九、内存管理

| 术语 | English | 说明 |
|------|---------|------|
| 栈 | stack | 自动存储期，函数调用时分配，**后进先出，容量有限** |
| 堆 | heap | 动态分配的区域，**由程序员手工配平** |
| 静态存储区 | static storage | 全局变量与 `static` 局部变量，**生命周期是整个程序** |
| 内存分配 | allocation | `malloc`/`calloc`/`realloc`/`free` |
| 内存泄漏 | memory leak | 分配了但没释放，进程退出才归还 |
| 双重释放 | double free | 同一块内存 `free` 两次，**可能直接摧毁整个堆** |
| 越界 | out-of-bounds | 访问数组边界之外的内存，**是未定义行为而非崩溃** |
| 野指针写 | wild write | 通过失效指针改内存，破坏对象头，往往比读更难查 |

---

## 十、错误处理

| 术语 | English | 说明 |
|------|---------|------|
| 错误码 | error code | 用返回值表示失败，如 `-1`、`NULL`、`errno` |
| errno | errno | 库函数失败后设置的错误码，**成功时不应清零** |
| 断言 | assertion | `assert(cond)`，失败即终止。**NDEBUG 下被编译掉**，不能用于校验外部输入 |
| 防御性编程 | defensive programming | 校验参数、检查返回值、不信任外部输入 |
| 未定义行为 | undefined behavior (UB) | 标准不作任何规定的行为。**编译器可以生成任意代码** |
| goto cleanup | goto cleanup | 用 `goto` 跳到统一的清理段落，避免嵌套 if 漏掉释放 |

---

## 十一、标准库与标准

| 术语 | English | 说明 |
|------|---------|------|
| 标准库 | standard library | ISO C 规定的库，`stdio`/`string`/`stdlib`/`math`/`time`/`ctype`/`assert`/`errno` |
| 文件流 | stream (`FILE`) | stdio 的对象模型，`stdin`/`stdout`/`stderr` 是三个预定义流 |
| 文件描述符 | file descriptor (fd) | POSIX 层的整数句柄，stdio 底层用的就是它 |
| 缓冲 | buffering | I/O 的批量缓存策略，影响 `fflush`/`setvbuf` 的必要性 |
| C89/C90 | ANSI C / ISO C90 | 第一个标准，**没有 `//` 注释、没有 for 内声明** |
| C99 | C99 | `//` 注释、VLA、`_Bool`、`long long`、复合字面量、指定初始化器、`snprintf` |
| C11 | C11 | `_Generic`、`_Static_assert`、`_Alignas`、`_Atomic`、匿名结构体、`_Thread_local` |
| C17 | C17 | **与 C11 语言层面完全等价**，只有缺陷修订。本项目基准 |
| C23 | C23 | `nullptr`、`constexpr`、`auto` 推导、`typeof`、`#embed`、二进制字面量 |
| GNU 扩展 | GNU extension | `__attribute__`、`typeof`、语句表达式等非标准特性，Linux 内核大量使用 |
| 交叉编译 | cross-compilation | 在 x86 上编译给 ARM 用，工具是 `arm-none-eabi-gcc` |

---

## 十二、工具链

| 术语 | English | 说明 |
|------|---------|------|
| 工具链 | toolchain | 编译器 + 汇编器 + 链接器 + 库 + 调试器的集合 |
| WSL | Windows Subsystem for Linux | **本项目在 Windows 上的主线**：在 WSL 里用 Linux 版 gcc |
| MinGW-w64 | MinGW-w64 | Windows 上的 gcc 移植。**ABI 与 MSVC 不兼容**，本项目不采用 |
| MSVC | Microsoft Visual C++ | Windows 原生编译器，仅在必须适配原生 ABI 时使用 |

---

## 相关文件

- 导航页（全部卡片）：[README_C.md](./README_C.md)
- 术语表对应的 C++ 版本（完整包含本表全部术语）：`../CPP/GLOSSARY_CPP.md`
- 标准版本与编译器扩展：见导航页「附 · 查阅型 Block」
