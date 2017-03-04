# C#的进化史

## LINQ

LINQ，查询表达式可以以一种声明风格对不同数据源创建查询。

### 过滤

```c#
List<Product> products = Product.GetSampleProducts();
var filtered = from Product p in products
    where p.Price > 10
    select p;
foreach (var product in filtered) {
    Console.WriteLine(product);
}
```

### 连接（joining）、过滤（filtering）、排序（ordering）、和投影（projecting）

```c#
List<Product> products = Product.GetSampleProducts();
List<Supplier> suppliers = Supplier.GetSampleSuppliers();

var filtered = from Product p in products
    join s in suppliers on p.SupplierId equals s.Id
    where p.Price > 10
    orderby  s.Name, p.Name
    select new {SupplierName = s.Name, ProductName = p.Name};
foreach (var v in filtered) {
    Console.WriteLine("Supplier={0}; Product={1}", v.SupplierName, v.ProductName);
}
Console.ReadKey();
```

### 查询XML

### LINQ to SQL

### COM和动态类型

### 与动态语言相互操作

```c#
ScriptEngine engine = Python.CreateEngine();
ScriptScope scope = engine.ExecuteFile("FindProducts.py");
dynamic products = scope.GetVariable("products");
foreach (dynamic product in products) {
    Console.WriteLine("{0}: {1}", product.Name, product.Price);
}
```

## 异步代码

在c#5中我们可以用**异步函数**来中断代码的执行，而不阻塞线程。

> 在Windows Forms中线程有两条金科玉律：不能阻塞UI线程，并且不能在任何其他线程中访问UI元素（除非是用一些明确的方法）。

下面的代码展示了Windows Forms应用程序中的一个处理按钮点击的方法，并根据产品ID显示产品信息。

```c#
private async void CheckProduct(object sender, EventArgs e) {
    try {
        productCheckButton.Enabled = false;
        string id = idInput.Text;

        Task<Product> productLookup = directory.LookupProductAsync(id);
        Task<int> stockLookup = warehouse.LookupStockLevelAsync(id);
        Product product = await productLookup;
        if(product == null) {
            return;
        }
        nameValue.Text = product.Name;
        priceValue.Text = product.Price.ToString("c");

        int stock = await stockLookup;
        stockValue.Text = stock.ToString();
    } finally {
        productCheckButton.Enabled = true;
    }
} 
```