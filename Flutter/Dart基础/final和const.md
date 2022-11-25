## final和const

 
### final和const共同点
#### 初始化后不能再赋值
```dart
final name = 'jack';
name = 'rose'; // error

const name = 'jack';
name = 'rose';

```

#### 类型声明

```dart
final String name = 'jack';
final name = 'jack';

const String name = 'rose';
const name = 'rose';
```

#### 不能和var同时使用
```dart
final var name = 'jack'; // error
const var name = 'rose';// error
```
### final 和 const 区别
##### 1、类中使用
* 类级别的常量，通常用static const
* 给final属性赋值，使用构造方法语法糖


```dart
class Person{
  // 类级别的常量，必须使用const修饰
  static const age = 10;

  
  final String name;
  // 给final的属性赋值，必须使用语法糖的格式，这样给name赋值，会在构造函数之前执行
  Person(this.name); // success

  /* 不能使用以下方法，给final属性赋值
  Person(String name){
    this.name = name;
  }
  */
}
```

##### 2、const 可以使用其他const常量来初始化
```dart
const width = 100;
const height = 50
const square = width * height;
```
##### 3、需要确定的值
```dart
final now = DateTime.now();// success 运行时能确定值
const now = DateTime.now();// error, 需要编译时有确定值
```
##### 4、不可变性可传递
const的不可变性是可传递的，final不是

```dart
final List nums =  [1,2,3];
nums[1] = 10; // success

const List nums = [1,2,3];
nums[1] = 10; // error
```


### 总结
* final 必须初始化，只能赋值一次，且不能修改，赋值可以是常量也可以是变量
* const必须初始化，且不能修改值，赋值必须是常量
* const必须根据可在编译时计算的数据创建它们，const对象无法访问运行时需要计算的任何内容
    * 例：1+2 是合法的const表达式，但是new DateTime.now()不是（合法的const表达式）；
* 使用const关键字声明的变量是隐式final变量，可以把const常量赋给final变量，反过来不可以
* final 和 const 可以与变量的数据类型一起使用，不可以与var 关键字一起使用


```
const 值必须在编译时知道，
const birth = '2019/08/01' // 初始化后无法更改
const birth = DateTime.now() 
// 报错 因为我们无法将运行时值分配给 const 变量
```


```
final 值必须在运行时知道，最终生成 final birth = getBirth()。初始化后无法更改
final birth = DateTime.now() // OK
```





