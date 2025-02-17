# Classes and Objects


# 1 Class
https://kotlinlang.org/docs/classes.html 
## Constructors﻿

### Primary constructor  
- 关键字 constructor，当没有annotations or visibility modifiers时，可以被省略。 
- private constructor() 表示不可见public constructor。

### Secondary constructors
- 关键字 constructor
- 可以有多个
- 没有提供任何constructor时，会自动生成 public primary constructor with no arguments

## Initializer blocks
- 关键字 ： init

## Property initializers

- 初始化的顺序？    
Primary constructor -> (Initializer blocks and Property initializers,按在body中的声明顺序 ) ->  Secondary constructors


## Abstract classes
- 关键字`abstract`
- Can override a non-abstract `open` member with an abstract one
- 关键字`open` 表示这个class 或 functions 可以被override

## Companion objects : TO DO 

# 2 Inheritance
https://kotlinlang.org/docs/inheritance.html

- Root class？  
All classes in Kotlin have a common superclass, `Any`

- 如何可以被继承？  
  By default, Kotlin classes are final – they can't be inherited.    
  Use `open` keyword to make a class inheritable.      
  final class 不可以被继承。

- 构造函数？    
  How derived class to initialize the base type?    
If the derived class has a primary constructor, the base class must be initialized in that primary constructor.    
If the derived class has no primary constructor, then each secondary constructor has to initialize the base type using the `super` keyword  

- Overriding methods？  
 `override` modifier 来重写`open` keyword 标注的fun。    
open 修饰符对final class 无效。    
用`final`修饰fun可以防止被重写。    

- Overriding properties?  
`override` modifier  

- Derived class initialization order  
first, the base class initialization is done as the first step :   
base class - Primary constructor    
->  base class - {Initializer blocks and Property initializers according to  defined order }   
-> base class - Secondary constructors      
then , derived class is  initialized:  
derived class - Primary constructor  
-> derived class - {Initializer blocks and Property   initializers according to  defined order }  
-> derived class - Secondary constructors  

-  Calling the superclass implementation  
`super` keyword.    
调用外部类的superclass implementation，用`super@Outer`。    
当 class 继承了多个实现时，用`super<Base>`指定 supertype name。    

# 3 Properties
https://kotlinlang.org/docs/properties.html#compile-time-constants

- Backing fields : 用 field identifier  
- Backing properties  

- Compile-time constants: 用`const val`.   
a top-level property, or a member of an object declaration or a companion object.    
https://www.jianshu.com/p/1fbba238cfa5 

  top-level property ?    
  把属性的声明不写在 class 里面。    

  top-level function?   
  把函数的声明不写在 class 里面。  

- Late-initialized properties and variables : 用 `lateinit var`   
 `::属性名称.isInitialized`用来 检查属性是否初始化。
 https://cloud.tencent.com/developer/article/2254148     

# 4 Interfaces
https://kotlinlang.org/docs/interfaces.html 
- keyword `interface`
- Interfaces中可以定义Properties

# 5 Functional (SAM) interfaces
- An interface with only one abstract method is called a `functional interface``, or a `Single Abstract Method (SAM) interface``.   
- `fun interface`  
TODO

# 6 Visibility modifiers
- visibility modifiers in Kotlin: private < protected < internal < and public(default).


# 7 Extensions
- Extension functions: 不使用继承的情况来一个类或一个接口的函数。    
- extension properties：不使用继承的情况来一个类或一个接口的属性。    
- Companion object extensions
- Declaring extensions as members

# 8 Data classes  
https://kotlinlang.org/docs/data-classes.html

- 使用`data class`
- 默认实现了    
.equals()/.hashCode() pair    
.componentN() functions    
.toString()  
.copy()  

# 9 Sealed classes and interfaces : TODO
https://kotlinlang.org/docs/sealed-classes.html  
- `sealed interface` and `sealed class`不能被扩展

# 10 Generics: in, out, where : TODO 
https://kotlinlang.org/docs/generics.html    
https://book.kotlincn.net/text/generics.html      
https://www.jianshu.com/p/c5ef8b30d768    
https://zhuanlan.zhihu.com/p/59349736  

- Generics type : in(下界) / out(上界) / in and out.    

- out 等价于Java `上界 <? extends T>`，accepct E or a subtype of E，适合于生产者。  
生产者指的是能用来读取的对象。=> Producer out to read.   
E.g., `Foo<out T : TUpper>` : Foo<*> equals to `Foo<out TUpper>` 


- in  等价于Java `下界 <? super T>`，accepct E or a supertypes of E。  
适用于消费者。消费者指的是用来写入的对象。=> Consumer in to write.  
E.g., `Foo<in T>` : Foo<*> equals to `Foo<in Nothing>`  

- in and out  
=> Producer out to read, and Consumer in to write.    
E.g., `Foo<T : TUpper>`: Foo<*> equals to `Foo<out TUpper>`for reading and `Foo<in Nothing>`  for writing. 

# 11 Nested and inner classes
https://kotlinlang.org/docs/inline-classes.html
https://book.kotlincn.net/text/nested-classes.html


# 12 Enum classes
https://kotlinlang.org/docs/enum-classes.html
- `enum`
-  自带属性：properties: `name` and `ordinal`
- accessed in a generic way ： `enumValues<T>()` and `enumValueOf<T>()` functions

# 13 Inline value classes

# 14 Object expressions and declarations

## Companion objects
- `companion object` 相当于  factory method
# 15 Delegation
# 16 Delegated properties
# 17 Type aliases