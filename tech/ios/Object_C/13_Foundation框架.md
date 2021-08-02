# Foundation 框架

- Foundation 文档

  (1) Developer Document  
  (2) 在 XCode 编辑文件时，当光标定位于代码上，按 Option 键并单击，调出概要面板。  
  (3) Online 文档  
  https://developer.apple.com/documentation

- 头文件

  Foundation 框架包含大量的类、方法和函数。在 OS X 下，大约有 125 个可用的头文件，通过下面的语句简单进行导入。

  ```c
  // Foundation.h 导入了Foundation所有的头文件。
  #import <Foundation/Foundation.h>
  ```

  TBD:  
  (1) 使用这条语句会明显增加程序的编译时间。然后，使用预编译的头文件可以避免额外的时间开销。  
  预编译的头文件是编译器预先处理过的文件。通常所有 XCode 项目都会受益于预编译的头文件。  
  (2) XCode5 中，"模块"的新功能能够帮助加速编译，以及帮助避免应用程序中使用不同的库出现的命名冲突。

- Foundation 集合只能存储对象，不能存储像 int 等基本数据类型。  
  Foundation 集合时，使用 NSNumber 对象数组，而不是 int 数组。
- Foundation 集合不能直接存储结构。可以用 NSValue 包装结构，然后再存储在集合中。

# 1 数字

`_13_main_4_number.m`

## NSNumber

- int、float、long 等它们是基本数据类型，不是对象。若需要对象，使用 NSNumber。
- 使用初始值创建 NSNumber 对象。
- 允许通过`@`表达式创建 NSNumber 对象。

## NSDecimalNumber

## NSInteger、NSUInteger

NSInteger、NSUInteger 不是一个对象，是基本数据类型的 typedef。  
NSInteger 是 64 位的 long，或 32 位的 int。

```c
#if __LP64__ || 0 || NS_BUILD_32_LIKE_64
typedef long NSInteger;
typedef unsigned long NSUInteger;
#else
typedef int NSInteger;
typedef unsigned int NSUInteger;
#endif
```

```c
NSInteger myInt = [intNumber integerValue];
// 将变量转换为long，并使用%li，目的是确保值能够传递而且正确显示，即使编译后是32位架构的。
NSLog(@"%li",(long)myInt);   // 100
```

![oc_NSNumber](https://yingvickycao.github.io/img/ios/oc_NSNumber.jpg)

# 2 字符串

## 不可变对象 与 可变对象

不可变对象：内容不可更改。  
可变对象 ： 内容可更改。

## NSString

`_13_main_4_string_1.m`  
`_13_main_4_string_2.m`

不可变对象。  
NSString 虽然是不可变对象，但是可以改变指向。

- NSLog 不仅仅可以显示 NSString，还可以显示其他对象

```c
NSString *str = @"Programming is fun";  // Programming is fun
NSLog(@"%@",str);

// NSLog 不仅仅可以显示NSString，还可以显示其他对象
NSNumber *intNumber = @100;
NSLog(@"%@",intNumber); // 100
```

- 重写对象的 description 方法

```c
NSString *s2 = “abc”:指向一个不可变的地址.
```

C 样式字符串由 char 字符组成。

NString 对象由 unichar 字符组成。  
unichar 字符是符合 Unicode 标准的多字节字符。  
目前 unichar 字符占 16 位，但 Unicode 标准规定的字符比这个尺寸大。所以在将来 unichar 字符可能大于 16 位，所以不要对 Unicode 字符的大小进行假设。

- 判断是否相等
- 比较大小  
  与 C 语言一样，按位比较
- get substring  
  substringFromIndex:[index,end]  
  substringToIndex:[0,index)  
  substringWithRange:[index, index + len -1 ]
- 是否包含 string  
   rangeOfString
- NString <=> UTF-8 NSData（二进制）：用于提交服务器，或者文件读写。  
  服务器接受的数据一般是 UTF-8。

![oc_NSString](https://yingvickycao.github.io/img/ios/oc_NSString.jpg)

## NSMutableString

可变的对象

NSMutableString 是 NSString 的子类

- 常见操作  
  增  
  删  
  改  
  查:同 NString

![oc_NSMutableString](https://yingvickycao.github.io/img/ios/oc_NSMutableString.jpg)

# 3 数组

`AddressBook.m`  
`_13_3_array_main_4_AddressBook.m`  
![oc_NSArray_NSMutableArray](https://yingvickycao.github.io/img/ios/oc_NSArray_NSMutableArray.jpg)

## `@property(copy,nonatomic, strong)`

`AddressCard.m`  
`_13_3_array_main_4_AddressCard2.m`

- `copy` vs `assign`  
  assign ：属性的默认关键字。生成的方法不会生成副本，仅仅进行赋值。  
  copy ： 在 setter 方法内生成实参变量的副本。

- `atomic` vs `nonatomic`  
  系统自动生成的 getter/setter 方法不一样。如果你自己写 getter/setter，那 atomic/nonatomic/retain/assign/copy 这些关键字只起提示作用，写不写都一样。  
  atomic ：属性的默认关键字。与 nonatomic 相比，速度较慢，但线程安全。  
  nonatomic ：与 atomic 相比，速度较快，但线程不安全。

```c
- (void)setName:(NSString *)theName andEmail:(NSString *)theEmail{
// 不可以：这种写法绕过了setter方法，直接为实例变量赋了参数的值。
//  name = theName;
//  email = theEmail;


// 可以:这种写法： self.name = theName 等价于[self setName: theName],使用了seter方法为实例变量赋值。
self.name = theName;
self.email = theEmail;
}
```

## 遍历数组

```c
// 遍历数组：快速遍历
- (void)list{
    for (AddressCard_2 *theCard in book) {
        NSLog(@"%-20s     %-20s", [theCard.name UTF8String], [theCard.email UTF8String]);
    }
}

// 遍历数组：enumerateObjectsUsingBlock
- (void)list2{
    [book enumerateObjectsUsingBlock:^(AddressCard_2 *theCard,NSUInteger idx,BOOL *stop){
        NSLog(@"%-20s     %-20s", [theCard.name UTF8String], [theCard.email UTF8String]);
    }];
}
```

### 性能 : ForLoop vs For-in vs enumerateObjectsUsingBlock

`_13_3_array_main_4_loop.m`

- 遍历一个数组的时候使用 For-in 最快
- 对于数组，通过 Value 找出 Index，enumerateObjectsWithOptions 最快。

## removeObjectIdenticalTo vs removeObject

- removeObjectIdenticalTo : 按内存地址。内存地址相同，则视为同一个对象，会删除
- removeObject ： 通过 isEqual 比较内容。只有内容完全相同，才删除。

## 数组排序

- 使用 selector 排序名：需要为比较的对象添加比较方法

```c
// AddressBook.m
[book sortUsingSelector:@selector(compareNames:)];

// AddressCard_2.m
- (NSComparisonResult)compareNames:(id)element{
    // 默认按升序排序
    // return [name compare:[element name]];
    // 按降序排序
    return [name compare:[element name]] == kCFCompareLessThan;
}
```

- 使用区块排序 NSMutableArray：不需要为比较的对象添加比较方法

```c
// AddressBook.m
[book sortUsingComparator:^NSComparisonResult(id  _Nonnull obj1, id  _Nonnull obj2) {
      return [[obj1 name] compare: [obj2 name]];
  }];
```

## NSValue

`_13_3_Array_main_4_NSValue.m`  
用 NSValue 类将结构转化为对象，并可以将它存储在结合中。

包装(wraping)：将结构转化为对象。
展开(unwraping)：从对象中解出基本类型。

![oc_NSValue](https://yingvickycao.github.io/img/ios/oc_NSValue.jpg)

# 4 字典（Dictionary）

- 类似与 Java 的 map
- 字典是由键-对象组成的数据集合。
- 字典是无序的。从字典中取出的键也是无序的。
- 如何以字母顺序显示词典中的内容？先取出词典中的所有键并排序，然后按照排好的键从字典中取出所有的值
- key 不能是 nil
- key 可以相同，但 value 会覆盖前面设置的值。
- key 是单值，通常是字符串，也可以是其他对象类型。
- value 可以是任何对象类型，但不能是 nil。  
  测试：value 虽然可以是 nil，但是根本检索不出来，就像没有存一样。

![os_dictionary](https://yingvickycao.github.io/img/ios/os_dictionary.jpg)

# 性能：For-in vs enumerateObjectsUsingBlock

`_13_5_dictionary_main_4_loop.m`  
遍历字典，enumerateKeysAndObjectsUsingBlock 速度更快，也更优雅。

# 5 集合
