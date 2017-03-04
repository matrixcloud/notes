# 泛型

## 引用类型约束

用于确保使用的类型实参是引用类型（它表示成``` T : class ```, 且必须是为类型参数指定的第一个约束）。

```c#
struct RefSample<T> where T : class
```

有效的封闭类型：

- RefSample<IDisposable>
- RefSample<string>
- RefSample<int[]>

无效的封闭类型：

- RefSample<Guid>
- RefSample<int>

## 值类型约束

这种约束表示成```T : struct```，可以确保使用的类型实参是值类型，包括枚举（enums）。

```c#
class ValSample<T> where T : struct
```

有效的封闭类型：

- ValSample<int>
- ValSample<FileMode>

无效的封闭类型：

- ValSample<object>
- ValSample<StringBuilder>

## 构造函数类型约束

构造函数类型约束表示成```T : new()```，必须是所有类型参数的最后一个约束，它检查类型实参是否有一个可用于创建类型实例的无参构造函数。

```c#
public T CreateInstance<T> () where T : new() {
    return new T();
}
```
