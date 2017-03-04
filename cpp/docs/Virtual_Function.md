## 虚函数

以下AA类继承至类A

```cpp
void A::f() {
    cout << "A::f()" << endl;
}

void AA::f() {
    cout << "AA::f()" << endl;
}

// 例一

A* a = new A;
AA* aa = new AA;
a->f(); // A::f()
aa->f(); // AA::f()

// 例二

A* a = new AA();
a->f(); // A::f()
```

在这里，我们希望例二输出AA::f()，因此c++提供了虚函数实现机制：``` virtual void f(); ```

## 虚析构函数

```cpp
A *a = new AA
delete a; // 调用了A的析构函数，需要定义虚析构函数。
```

## 纯虚构函数

纯虚构函数只是在定义上有声明，而没有实现。拥有纯虚构函数的类被称为抽象类。

``` virtual void f(int n)=0; ```