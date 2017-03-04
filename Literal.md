## 字面量

字面量（literal），是指c++中直接书写的数字、字符和字符串，如：0x3f, '+', 1.23e-15L, "hello", L"word"。

- 整型字面量
- 浮点型字面量
- 字符型字面量
- 字符型串字面量
- 枚举型字面量
- 布尔型字面量
- 指针型字面量
- 用户自定义字面量

## 字符型字面量

字符型字面量的值等于该字符在**执行字符集（execution character set）**的编码值。

实际上，编译器在token分析阶段，通常会把字符与字符串在源文件中的编码串转换为指定或执行字符集的编码串。

如：源文件是*Latin-1*编码，执行字符集为utf-8，则``` char ch = 'ö' ```中的字符值将被编译器从*Latin-1*编码的单字节的0xD6自动转换成utf-8编码的双字节的0xC3B6，在**目标文件**与**可执行文件**中的字符ch的值是'ö'的utf-8编码值的最后一个字节值即0xB6。


## 字符串字面量

由```R"<someChars>()<someChars> ```括起来的字符串字面量叫做**raw string literal**，这可以避免大量使用反斜线转义字符造成的**倾斜牙签综合症**，特别适合**正则表达式**的模式字符串。

```cpp
std::string filePath = R"(c:\foo\bar.txt)";
std::regex re{ R"abc(s/"\([^"]*\)"/'\1'/g)abc" }; // 如果字符串包含了)"这两个字符串的组合，可选别的分节符，如abc。
```

## 指针型字面常量

c++11定义了一个字面量nullptr，其类型是std::nullptr_t。

## 自定义的字面量

c++11新增了用户定义的字面量（user defined literal）。由用户给出字面常量的后缀，并给出字面量运算符函数的定义。编译器可以在运行期和编译期把带有这样后缀的整形、浮点型、字符型、字符串型的字面量通过调用用户的字面量运算函数，生成置顶数据类型的对象。

```cpp
struct S {
    int value;
};

S operator "" _mysuffix(unsigned long long v) {
    S s;
    s.value = (int)v;
    return s;
}

int main(){
    S s;
    s = 101_mysuffix; // 这里的101是类型S的字面量
    std::cout << s.value << std::endl; // 输出：101
    return 0;
}
```

## 字面常量

字面常量（literal constant），是c/c++的此法上的概念（lexical conventions），是指源程序中表示固定值的符号（token）。

> 由于字符型字面量可能不属于c/c++的token的字符范围，这就需要用反斜线\开始的**转义序列**来表示一个字符值。

> [引用wikipedia](https://zh.wikipedia.org/wiki/字面常量_(C语言))

## 总结

- 字面量的类型由实际数值范围和后缀决定。