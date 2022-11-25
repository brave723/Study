# 异常 Exception

> 一个成熟的计算机语言必须有异常机制, 因为它可以帮助我们发现问题和解决问题。

### 抛出异常
`throw` 是 Dart 中抛出异常的关键字，接下来我们来模拟抛出一个异常,

```dart
main(List<String> args) {
    test();
}

void test() {
  throw Exception('异常出现了');
}
```
在输出结果中可以看到运行报错，并且提示`异常出现了`, 在`Exception()`构造方法中可以传入字符串类型的消息。

### 捕获异常
`try catch`是 Dart 中捕捉异常的关键字，来看一下如何捕捉异常：

```dart
main(List<String> args) {
  try {
    test();
  } catch (e) {
    print('捕捉到了一个异常, 异常消息: ${e.toString()}');
    print('处理异常...');
  }
}

void test() {
  throw UnsupportedError('不支持该次操作');
}
//输出结果
//捕捉到了一个异常, 异常消息: Unsupported operation: 不支持该次操作
//处理异常...
```

还可以同时用到`finally`关键字, 它会在`catch`代码执行完以后必定执行
```dart
main(List<String> args) {
  try {
    test();
  } catch (e) {
    print('捕捉到了一个异常, 异常消息: ${e.toString()}');
    print('处理异常...');
  }finally{
    print('处理完毕后，finally最终执行');
  }
}

void test() {
  throw UnsupportedError('不支持该次操作');
}
//输出结果
//捕捉到了一个异常, 异常消息: Unsupported operation: 不支持该次操作
//处理异常...
//处理完毕后，finally最终执行
```

在 Dart 中，`throw`不止能抛出`Exception` 和`Error`, 还可以抛出任意对象。

> 这里简单讲下`Exception`和`Error`其实没有本质性的区别，只是在严重程度上做了一下区分。

我们来测试下字符串类型
```dart
main(List<String> args) {
  try {
    test();
  } catch (e) {
    print('捕捉到了一个异常, 异常消息: ${e.toString()}');
    print('处理异常...');
  }
  
}

void test() {
  throw '我是一个字符串';
}
//输出结果
//捕捉到了一个异常, 异常消息: 我是一个字符串
//处理异常...
```
我们来测试下整型
```dart
main(List<String> args) {
  try {
    test();
  } catch (e) {
    print('捕捉到了一个异常, 异常消息: ${e.toString()}');
    print('处理异常...');
  }
}

void test() {
  throw 123;
}
//输出结果
//捕捉到了一个异常, 异常消息: 123
//处理异常...
```
> 这样的特性得利于 Dart 是面向对象语言，所有的一切都是对象。

### 多异常捕获

日常开发中很常见的是，一段代码可能产生多个不同类型的异常，这个时候需要我们进行多异常的捕获及处理。

需要用到`on`关键字：

```dart
main(List<String> args) {
  try {
    testA();
    testB();
    testC();
  } on UnsupportedError {
    print('处理不支持的错误');
  } on WrongNameException {
    print('处理错误名称异常');
  } on NoNetWorkException {
    print('处理无网络异常');
  } catch (e) {
    print('处理其他类型异常');
  }
}

void testA() {
  throw UnsupportedError('不支持该次操作');
}

void testB() {
  throw WrongNameException();
}

void testC() {
  throw NoNetWorkException();
}

class WrongNameException implements Exception {}

class NoNetWorkException implements Exception {}

```

### 再抛出异常

有些特定的情况可能需要对异常进行部分处理，处理后需要重新抛出异常给到外部的函数继续处理，这时候要用到`rethrow`关键字

```dart
void misbehave() {
  try {
    dynamic foo = true;
    print(foo++); // Runtime error
  } catch (e) {
    print('misbehave() 部分处理了 ${e.runtimeType}.');
    rethrow; 
  }
}

void main() {
  try {
    misbehave();
  } catch (e) {
    print('main() 完成处理 ${e.runtimeType}.');
  }
}
```

