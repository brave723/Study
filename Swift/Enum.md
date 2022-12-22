# Enum

#### 枚举的内存布局
在Swift中查看内存占用大小及对齐方式使用`MemoryLayout`：
* size：实际用到的空间大小
* stride: 分配占用的空间大小
* alignment：内存对齐方式

Int在内存中占用8个字节，内存对齐数是8：
```Swift
MemoryLayout<Int>.size // 输出：8
MemoryLayout<Int>.stride // 输出：8
MemoryLayout<Int>.alignment // 输出：8
```

### 关联值和原始值

```Swift
enum Password {
    case number(Int, Int, Int, Int)
    case other
}

var pwd = Password.number(1, 2, 2, 3)

MemoryLayout.size(ofValue: pwd) // 输出：33
MemoryLayout.stride(ofValue: pwd) // 输出：40
MemoryLayout.alignment(ofValue: pwd) // 输出：8
```
