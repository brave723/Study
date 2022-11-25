# 库（libraries）

在日常的项目中，库是很常见的一种管理代码方式。

我们可以把一些可复用的代码或者模块做成一个 lib，供自己将来或者其他人使用。

## 使用

`import` 关键字就是导入一个指定的，例如导入 dart 的 `math 库`

```dart
import 'dart:math';
import 'package:sqflite/sqflite.dart';
```

- `import` 必须指定`唯一的scheme`
- dart 内置库的格式是`dart:scheme`
- 其他的库可以是文件路径或者`package:scheme`
- `package:scheme` 开头的都是由包管理器提供的库

### 指定库的别名

日常开发中可能会遇到导入的两个库都有同一个类名导致冲突，库的别名就可以用来处理这类的问题，例如

```dart
import 'package:lib1/lib1.dart';
import 'package:lib2/lib2.dart' as lib2;

// 使用 lib1 中的 Element 
Element element1 = Element();

// 使用 lib2 中的 Element 
lib2.Element element2 = lib2.Element();

```

- `lib1` 与 `lib2` 中都存在 Element
- 定义 `lib2` 别名，使用时以`别名前缀`的方式来引用 `Element`

### 选择性导入

```dart
// 只导入 foo
import 'package:lib1/lib1.dart' show foo;

// 除了 foo 其他都导入
import 'package:lib2/lib2.dart' hide foo;
```

### 延迟加载

顾名思义，当用到的时候才去加载库，但是这个写法只有在 dart2js 中才有效，使用关键字 `deferred as` 

```dart
import 'package:greetings/hello.dart' deferred as hello;
```

使用时, 用到`loadLibrary()`

```dart
Future greet() async {
  await hello.loadLibrary();
  hello.printGreeting();
}
```

- `await` 关键字会等待库被加载好以后，才去执行下一句代码
- `loadLibrary()` 多次执行也只会加载一次库
- 被延迟加载的库里的常量不再是‘常量’ ，等待延迟库被加载完以后才能调用
- 延迟库里的类要等到加载完以后才能使用，所以接口库建议单独提取出来，以免影响使用
- `loadLibrary()` 会返回一个 `Future` 对象

## 创建

库是所有计算机语言的生态系统，有句话说的好，不要重复造轮子，要站在巨人的肩膀上。当然这并不是说你可以不用搞懂库里面的原理。

### 一个库包由什么组成

下面的演示图展示了一个最简单的库包的结构：
![](https://user-gold-cdn.xitu.io/2019/9/21/16d53f5240eead8d?w=284&h=72&f=png&s=2282)

一个库包最少需要：

**pubspec 文件**
`pubspec.yaml`文件对于一个库和一个应用包来说都是相同的--没有特殊的设计来区分一个包是一个库。

**lib 目录**
就像你想的，lib 目录下放的是这个库的代码，并且对于其他包是公开的。你可以在 lib 目录下创建任何层级。为了方便，实现的代码是放在 lib/src下的。在 lib/src下的代码可以考虑设置成私有的；其他包不需要导入 `src/...`。为了让你在 lib/src 下的 APIs 公开， 你可以从一个直接在 lib 目录下的文件中导出`lib/src`下的文件。

> 注意：未指定`library`指令时，会根据每个库的路径和文件名为它们生成一个唯一标记。 因此，我们建议你从代码中省略`library`指令，除非你打算生成[库级文档](https://dart.dev/guides/libraries/create-library-packages#documenting-a-library)。

### 组织一个库包

创建小型的单个库（称为迷你库）时，库包最容易维护，扩展和测试。 在大多数情况下，每个类都应位于自己的微型库中，除非你遇到两个类紧密耦合的情况。

> 注意：你可能听说过`part`指令，该指令可将库拆分为多个Dart文件。 我们建议你避免使用`part`而是创建小型库。

在lib, lib/<package-name>.dart下直接创建一个**main**库文件，该文件将导出所有公共API。 这使用户可以通过导入单个文件来获得库的所有功能。

lib目录可能还包括其他可导入的非src库。 例如，也许你的主库跨平台工作，但是你创建了依赖`dart:io`或`dart:html`的单独的库。 某些软件包具有单独的库，但如果没有主库，则应使用前缀导入。

让我们看一下现实世界中库包的结构：shelf。 [shelf](https://github.com/dart-lang/shelf)包提供了一种使用Dart创建Web服务器的简便方法，并以Dart库包常用的结构进行布局：
![](https://user-gold-cdn.xitu.io/2019/9/21/16d53f552c0bdd8b?w=658&h=207&f=png&s=8973)
直接在lib下，主库文件，`shelf.dart`，从lib/src导出几个文件：
```dart
export 'src/cascade.dart';
export 'src/handler.dart';
export 'src/handlers/logger.dart';
export 'src/hijack_exception.dart';
export 'src/middleware.dart';
export 'src/pipeline.dart';
export 'src/request.dart';
export 'src/response.dart';
export 'src/server.dart';
export 'src/server_handler.dart';
```
shelf包也包含了一个mini 库：`shelf_io`。这个适配器处理了从 dart:io 中的 HttpRequest 对象。

> Web应用程序提示：为了在使用dartdevc开发时获得最佳性能，请将实现文件放在`/lib/src`下，而不是在`/lib`下的其他地方。 另外，避免导入`package:package_name/src/....`

### 导入库文件

导入一个库文件时，可以使用`package:`直接指定它的 URI
```dart
import 'package:utilities/utilities.dart';
```

无论文件是不是在 lib 目录内外，都可以用相对路径导入一个库，但是你必须使用`package:`。如果遇到有疑问的时候，直接使用`package:`指令在所有情况下都管用。

下图显示了如何从lib和web导入`lib/foo/a.dart`。
![](https://user-gold-cdn.xitu.io/2019/9/21/16d53f524af46e03?w=615&h=292&f=png&s=8632)

> 注意：尽管lib图形使用相对导入（`import'../foo/a.dart'`）显示`lib/bar/b.dart`，但它可以改用`package:`指令（`import'package：my_package/foo/ a.dart'`）。

### 提供额外的文件
设计良好的库包易于测试。 建议你使用[test](https://github.com/dart-lang/test)包编写测试，并将测试代码放在包顶部的`test`目录中。

如果你创建任何供公众使用的命令行工具，请将其放置在`bin`目录中，该目录是公共的。 使用[pub global activate](https://dart.dev/tools/pub/cmd/pub-global#activating-a-package)启用从命令行运行工具的功能。 在pubspec的[可执行文件部分](https://dart.dev/tools/pub/pubspec#executables)列出该工具后，用户无需调用[pub global run](https://dart.dev/tools/pub/cmd/pub-global#running-a-script-using-pub-global-run)就可以直接运行它。

提供一个如何使用你的库的例子会非常有用。 可以放在包的顶级目录`example`中。

你在开发过程中创建的任何非公共使用的工具或可执行文件都可以放入`tool`目录。

如果将库发布到Pub站点，则需要的其他文件（如README和CHANGELOG）在[发布包](https://dart.dev/tools/pub/publishing)中进行了描述。 有关如何组织包目录的更多信息，请参见[pub包布局约定](https://dart.dev/tools/pub/package-layout)。

### 为一个库写文档

你可以使用[dartdoc](https://github.com/dart-lang/dartdoc#dartdoc)工具为你的库生成API文档。 Dartdoc解析源以查找使用`///`语法的[文档注释](https://dart.dev/guides/language/effective-dart/documentation#doc-comments)：

```dart
/// 负责处理在 ui 中更新 badge 的事件
void updateBadge() {
  ...
}
```
有关生成的文档的示例，请参阅[shelf文档](https://pub.dev/documentation/shelf/latest)。

> 注意：要在生成的文档中包括任何库级别的文档，必须指定`库`指令。 请参阅[问题1082](https://github.com/dart-lang/dartdoc/issues/1082)。

### 分发开源库
如果你的库是开源的，我们建议在[Pub网站](https://pub.dev/)上共享它。 要发布或更新库，请使用[pub publish](https://dart.dev/tools/pub/cmd/pub-lish)，它会上传你的软件包并创建或更新其页面。 例如，请参阅[shelf包](https://pub.dev/packages/shelf)页面。 有关如何准备要[发布包](https://dart.dev/tools/pub/publishing)的详细信息，请参见发布包。

发布网站不仅托管你的软件包，而且还生成并托管你的软件包的API参考文档。 软件包的**About**框中提供了最新生成的文档的链接； 例如，请参阅书架包装的[API文档](https://pub.dev/documentation/shelf)。 程序包页面的**Versions**标签中的指向先前版本文档的链接。

为确保你的软件包的API文档在发布网站上看起来不错，请按照以下步骤操作：

- 在发布包之前，请运行[dartdoc](https://github.com/dart-lang/dartdoc#dartdoc)工具，以确保你的文档生成成功并且外观符合预期。
- 发布包后，请检查**Versions**选项卡，以确保文档已成功生成。
- 如果文档根本没有生成，请在**Versions**标签中单击**failed**以查看dartdoc输出。







