# Map

map是每种计算机语言中必备的数据格式，它以键值对的方式存储。
接下来老师会带你们了解一下Dart中的map

首先讲一下如何创建一个 map，创建一个空map对象有以下几种方式

```dart
  var empty1 = {};
  var empty2 = Map();
  Map empty3 = {};
  Map empty4 = Map();
```

创建一个有值 map 对象有以下几种方式

```dart
var noempty1 = {"姓名": "李雷", "年龄": 10, "身高": 142.5};
var noempty2 = Map();
noempty2["姓名"] = "韩梅梅";
noempty2["年龄"] = 10;
noempty2["身高"] = 140.5;
//跟创建空 map 一样，这里的 var 都能替换成具体类型
//Map noempty1 = ...
//Map noempty2 = ...
```

上面的例子都没有为 Map 的 Key 和 Value 申明具体的类型，
当我们为 Map 申明具体类型时，Key 和 Value 必须传入指定类型，否则编译器会报错
这个将在后续泛型课程中讲到，这里就不再做过多叙述

接下来讲一下如何用 Map 进行增删改查
我们先来定义一个通讯录Map, 这个 Map 的key是号码名称，value 是具体的电话号码

```dart
var phoneMap = {"移动": "10086", "电信": "10000", "联通": "10010"};
```

接下来老师通过”移动“的名称来快速查找到移动公司的电话

```dart
var num = phoneMap["移动"];
print(num);
```
  
因为老师被移动乱扣费，所以气的决定把它号码删了

```dart
phoneMap.remove("移动");
print(phoneMap);
```

但老师转念一想，宽带还是移动的，所以还是把它加回来

```dart
phoneMap["移动"] = "10080";
print(phoneMap);
```

老师发现移动号码不小心加错了，需要修改下，方法跟上面一样

```dart
phoneMap["移动"] = "10086";
print(phoneMap);
```

那有人会问，一个一个加太麻烦了，能不能一下子加多个啊？当然可以

```dart
phoneMap.addAll({"老姜家":"12345","老王家":"54321"});
print(phoneMap);
```

好不容易录了一份通讯录，老师想直接分享给其他人，那怎么复制 Map 呢？来看下代码

```dart
var copyOne = Map.from(phoneMap);
print("copyOne: ${copyOne}");
```

老师想把旧手机卖了，那通讯录得一条条删太麻烦了呀，想直接一次清空？来看下代码

```dart
phoneMap.clear();
print(phoneMap);
```

map 的讲解就到这了，接下去。。。




