# 类 Class

## 如何创建一个类

首先我们来创建一个最简单的类

```dart
class Person {
  String name;
  int age;
  Person(this.name, this.age){ 
  }
}

main(List<String> args) {
  var boyA = Person('Peter', 20);
  print(boyA.name);
}
//输出结果：
//Peter
```
- 用 `class` 关键字来定义一个类，名字叫`Person`
- `name` 和 `age` 都是 `Person` 类的成员变量
- `Person(this.name, this.age)` 是 Person 类的构造函数。
- `Person('Peter',20)` 创建了一个`Person`实例
- 为`boyA` 赋值了这个`Person`实例
- 随即可以使用 `boyA`变量获取实例对象中的成员变量`name`

### 类的成员

从上面的例子中，我们可以看到一个类中可以存在两种成员，分别是
- 函数
- 数据

> 我们也可以称之为：**方法和变量**。

调用类的方法可以获取到成员变量，这是类与类之间相互通信，或者调用的常用手段。

Dart 中使用`_`下划线开头的成员变量或者函数方法，表示私有的，即外部访问不了该变量或者函数。

在`person.dart`中定义一个`Person`类

```dart
class Person {
  String _name;
  int age;
  Person(this._name, this.age) {}

  _printAge(){
    print(age);
  }
}
```
在另一个文件中引入`person.dart`文件

```dart
import 'person.dart';

main(List<String> args) {
  var boyA = Person('Jack', 18);
  print(boyA._name);// 编译器报错
  print(boyA._printAge());// 编译器报错
}
```


**通常我们会遇到一些变量未赋值，或者赋值为空的情况，调用`null`的方法或成员变量时，程序就会报错终止**

```dart
class Person {
  String name;
  int age;
  Person(this.name, this.age) {}
  static Person createPerson() {
    return null;
  }
}

main(List<String> args) {
  var boyA = Person.createPerson();
  boyA.name = 'Peter';//此处报错
}

```

`?.`写法可以为我们带来防空操作，这和 kotlin 中的`?.`写法一样，只有当变量不为`null`时才会赋值。

```dart
class Person {
  String name;
  int age;
  Person(this.name, this.age) {}
  static Person createPerson() {
    return null;
  }
}

main(List<String> args) {
  var boyA = Person.createPerson();
  boyA?.name = 'Peter';// 运行 ok
}

```

### 构造函数

构造函数简单来讲就是类中会返回一个类实例的函数。一般构造函数有两种写法形式:

- `ClassName()`
- `ClassName.method()`

```dart
import 'dart:convert';

class Person {
  String name;
  int age;
  Person(this.name, this.age) {}

  static Person fromJson(String json) {
    var jsonCodec = JsonCodec();
    var jsonObj = jsonCodec.decode(json);// 此处 jsonObj是一个 LinkedHashMap类型
    print(jsonObj.runtimeType);
    return Person(jsonObj['name'],jsonObj['age']);
  }
}

main(List<String> args) {
  var boyA = Person('Jack', 18);
  var boyB = Person.fromJson('{"name":"Bob", "age": 21}');
  print(boyA.name);
  print(boyB.name);
}
//输出结果:
//_InternalLinkedHashMap<String, dynamic>
//Jack
//Bob
```

### 获取一个对象类型

运行代码中常常遇到返回`dynamic`类型，这是Dart 中的万能动态类型，表示可以返回任何对象

但是有利有弊，存储的时候特别方便，到了读取的时候我们要搞清楚具体的类型就很难。

所以代码中经常可以用到对象的`.runtimeType`这个属性， 可以告诉我们对象的具体类型，上述例子中已经使用到一次，相信你们已经对它有所了解。

### get set 方法

大多数的计算机语言设计中，get set 方法都是用来读取和设置对象的属性（或称为成员变量），在 Dart 中每个实例变量都有一个隐式的 get 方法，还可能有一个隐式的 set 方法，当然我们也可以通过`get 和 set`来显式的创建其他的属性。

```dart
class Rect {
  int width;
  int height;
  int get area => width * height;
  set area(int value) => height = (value/width).round();
  Rect(this.width, this.height){}
}

main(List<String> args) {
  var rectA = Rect(10,10);
  print(rectA.area);
  rectA.area = 50;
  print(rectA.height);
}
//输出结果：
//100
//5
```

- `width` 和 `height` 都有默认的 `setter getter`
- 为`area`创建显式的`get`方法 及 `set`方法
- 获取`area`属性时执行定义的显式`get`方法
- 设置`area`属性时执行定义的显示`set`方法

### 抽象类
用`abstract`关键字直接定义一个抽象类

```dart
abstract class Person {
  void printName();
  
  printNation(){
    print('china');
  }
}
```

- `printName()`并没有方法体，代表它是一个抽象方法
- `printNation()`有方法体，代表它是一个具体实现方法

### 隐式接口
每个类中都隐式定义了一个接口，这个接口包含了该类所有的实例成员及其实现的任何接口。
如果不想使用继承类这个方法去获取A类的所有 api，则可以使用`implements`去实现A类的接口。

```dart
class Person {
  // 在隐式接口中, 但只有在这个库中可见
  final _name;

  // 不在隐式接口中， 因为这是一个构造函数
  Person(this._name);

  // 在隐式接口中
  String greet(String who) => '你好, $who. 我的名字叫 $_name.';
}

// 一个Person 接口的实现
class IronMan implements Person {
  get _name => '钢铁侠';

  String greet(String who) => '你好, $who. 我是$_name';
}

String greetBob(Person person) => person.greet('Jerry');

void main() {
  print(greetBob(Person('Tom')));
  print(greetBob(IronMan()));
}
```
- 类的构造函数不在隐式的接口中, 成员变量及函数都在隐式接口中
- 使用`implements` 的`IronMan`必须实现`Person`类的所有成员变量及函数，否则编译器就会报错
- 通过`implements`后`IronMan`的类型也属于 `Person`类

`implements`可以多实现，比如：

```dart
class IronMan implements Person, Hero{
    ...
}
```

### 继承

上面在隐式接口中有提到`继承`, 继承就是创建一个子类, 使用`extends`关键字

```dart
class Food {
  canEat() {
    getEnergy();
  }

  getEnergy() {}
}

class Chocolate extends Food {
  @override
  canEat() {
    super.canEat();
    makeHappy();
  }

  makeHappy() {}
}
```

- 用`@override`关键字来覆写函数
- 用`super`关键字来调用父类的同名函数




### 枚举

用`enum`申明一个枚举类型

```dart
enum Color { red, green, blue }
```

枚举中的每个值都有一个`index`，它返回枚举声明中的索引值，该索引值会从零开始。 例如，第一个值具有索引0，第二个值具有索引1。

```dart
assert(Color.red.index == 0);
assert(Color.green.index == 1);
assert(Color.blue.index == 2);
```

如果要获取枚举中所有值的列表，可以使用`.values`来获取所有枚举值常量的列表。

```dart
List<Color> colors = Color.values;
assert(colors[2] == Color.blue);
```

你可以在switch语句中使用枚举，如果不处理所有枚举值，将收到警告：

```dart
var aColor = Color.blue;

switch (aColor) {
  case Color.red:
    print('Red as roses!');
    break;
  case Color.green:
    print('Green as grass!');
    break;
  default: // 不写这个 default，你会看到WARNING.
    print(aColor); // 'Color.blue'
}
```

枚举类型具有以下限制：

- 不能子类化，混合或实现枚举。
- 无法显式实例化枚举。

### mixins(混合)

Mixins是一种在多个类层次结构中重用类代码的方法。

使用混合的方法是`with`关键字后跟一个或多个mixin命名的类。 以下示例显示了两个使用mixins的类：

```dart
class Musician extends Performer with Musical {
  // ···
}

class Maestro extends Person
    with Musical, Aggressive, Demented {
  Maestro(String maestroName) {
    name = maestroName;
    canConduct = true;
  }
}
```

要定义一个mixin，首先创建一个`mixin`开头的Object扩展类，并且不能声明构造函数。例如：

```dart
mixin Musical {
  bool canPlayPiano = false;
  bool canCompose = false;
  bool canConduct = false;

  void entertainMe() {
    if (canPlayPiano) {
      print('Playing piano');
    } else if (canConduct) {
      print('Waving hands');
    } else {
      print('Humming to self');
    }
  }
}
```

要指定只有某些类型可以使用mixin  - 例如，mixin可以调用它没有定义的方法 - 使用`on`关键字来指定所需的超类：

```dart
mixin MusicalPerformer on Musician {
  // ···
}
```

### static 静态变量及静态方法

静态变量（类变量）通常用作类范围的状态和常量：

```dart
class Queue {
  static const initialCapacity = 16;
  // ···
}

void main() {
  assert(Queue.initialCapacity == 16);
}
```

静态变量在使用之前不会初始化。

> 注意：优先选择`lowerCamelCase`作为常量名称。


静态方法（类方法）不对实例进行操作，因此无法使用`this`。 例如：

```dart
import 'dart:math';

class Point {
  num x, y;
  Point(this.x, this.y);

  static num distanceBetween(Point a, Point b) {
    var dx = a.x - b.x;
    var dy = a.y - b.y;
    return sqrt(dx * dx + dy * dy);
  }
}

void main() {
  var a = Point(2, 2);
  var b = Point(4, 4);
  var distance = Point.distanceBetween(a, b);
  assert(2.8 < distance && distance < 2.9);
  print(distance);
}
```

> 注意：对于常用或广泛使用的实用程序和功能，请考虑使用顶级（top-level）函数而不是静态方法。

你可以使用静态方法作为编译时常量。 例如，可以将静态方法作为参数传递给常量构造函数。

