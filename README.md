## 构造函数

ctor主要用于初始化类对象的成员变量。当创建类的新对象时，构造函数会自动调用。

特点：

- 构造函数名必须与类名相同
- 不能有任何返回值（包括void）
- 可以有参数，因此构造函数支持重载

## 默认构造函数

不需要任何参数就能创建对象。可以是IDE自动生成，也可以是显式定义的

- 当类中没有定义任何构造函数时，IDE会自动生成一个默认构造函数
- 默认构造函数不会对内置类型的成员变量做初始化
- 可以使用`=default`显式声明默认构造函数

IDE自动生成默认构造函数的情况：

- 类中没有定义任何构造函数
```cpp
class Student {
    string name;
    int age;
    //IDE会自动生成默认构造函数：Student() = default;
};
```
- 所有成员变量都能默认初始化
```cpp
class SafeClass {
    int number;       //基本类型
    std::string text; //std::string是具有默认构造函数的模板类
    vector<int> data; //STL类型
    //IDE可以安全地生成默认构造函数
};
```
- 基类有可访问的默认构造函数

不会自动生成默认构造函数的情况：

- 定义了其他构造函数，IDE不再自动生成默认构造函数
- 类中包含引用成员或const成员时，因为它们声明时就必须初始化，所以不会自动生成默认构造函数
  - 在初始化引用成员或const成员时，必须使用初始化列表
```cpp
class NoDefault {
private:
    const int value; //const成员必须初始化
    int& ref;        //引用成员必须初始化
    //IDE无法生成默认构造函数
};
```
- 类中包含无默认构造函数的成员
```cpp
class NoDefault {
public:
    NoDefault(int x) : value(x) {}  //只有带参构造函数
    NoDefault() = delete;           //默认构造函数被禁用
private:
    int value;
};

class Container {
    NoDefault obj; //NoDefault没有默认构造函数
    string name;
    
    //IDE不会生成默认构造函数，必须手动编写构造函数来初始化obj
};
```
- 使用`=delete`显式禁用默认构造函数

## 委托构造函数

允许一个构造函数调用同一个类的另一个构造函数。可以减少代码重复

```cpp
class Mtx {
public:
    Mtx(int a) : num(a) { //初始化列表的ctor
        //可选的其他代码
    }
    //Mtx(double b)构造函数委托给Mtx(int a)构造函数来初始化num
    Mtx(double b) : Mtx(static_cast<int>(b)) {
        //可选的额外代码
    }
    //Mtx()构造函数委托给Mtx(0)构造函数来初始化num
    Mtx() : Mtx(0) {
        //可选的额外代码
    }
private:
    int num;
};
```

- 委托构造函数调用必须是构造函数初始化列表的第一位
```cpp
//错误
MyClass() : dval(3.14), MyClass(0) {}
```
- 委托构造函数调用只能出现在初始化列表中，不能在构造函数体内
- 不能循环委托构造函数，例如构造函数A委托给构造函数B，构造函数B又委托给构造函数A

## 
