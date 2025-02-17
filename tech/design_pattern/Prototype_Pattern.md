# 原型模式（ Prototype Pattern ）

# 1 定义

> 原型模型：当创建给定类的实例的过程很昂贵或者很复杂时，就使用原型模式

# 2 场景

在交互式角色扮演游戏中，可以让高级用户创建他们自己的怪兽，该怪兽拥有很多各种各样的随着场景变化而演化的特征。

# 3 How to use

原型模式允许通过复制现有的实例来创建新的实例。

- 如何复制？  
  clone()方法 或 反序列化。

- 拷贝分为浅拷贝(Shadow Clone) 和 深拷贝(Deep Clone)  
  浅拷贝：clone() by default is Shadow Slone.

  深拷贝:  
   Way 1 : custom clone()  
   Way 2 : Use Serializable to read byte stream

# 4 优点

- 向客户隐藏制造新实例的复杂性  
  PS:  
  如果 new 一个实例后，用户需要一个一个设置 N 多选项，是在太烦了。 e.g., JIRA 中 new 出 一个 Task。  
  但是，使用复制对象，就可以快速创建对象，用户只需要修改需要修改的地方就可以了。e.g., JIRA 中 clone 出 一个 Task

- 提供让客户能够产生未知类型对象的选项
- 在某些环境下，复制对象比创建新对象更有效。

# 5 用途

在一个复杂的类层次中，系统必须从其中许多类型创建对象时，可以考虑使用原型。  
在实际项目中，原型模式很少单独出现，一般是和工厂方法模式一起出现，通过 clone 的方法创建一个对象，然后由工厂方法提供给调用者。

# 6 缺点

对象的复制有时相当复杂
