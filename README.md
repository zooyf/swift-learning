# swift 3 访问控制

[官方文档入口](http:/developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/AccessControl.html)


## Modules and Source Files


Swift’s access control model is based on the concept of modules and source files.

A module is a single unit of code distribution—a framework or application that is built and shipped as a single unit and that can be imported by another module with Swift’s import keyword.

Each build target (such as an app bundle or framework) in Xcode is treated as a separate module in Swift. If you group together aspects of your app’s code as a stand-alone framework—perhaps to encapsulate and reuse that code across multiple applications—then everything you define within that framework will be part of a separate module when it is imported and used within an app, or when it is used within another framework.

A source file is a single Swift source code file within a module (in effect, a single file within an app or framework). Although it is common to define individual types in separate source files, a single source file can contain definitions for multiple types, functions, and so on.


## Access Levels (访问等级)

#### 1. open

   can be used anywhere

   可在任何地方使用


#### 2. public

   like open, differs below

   和open类似,区别在下面描述

#### 3. internal ``Default Access Level``

   used within the module. 

   在模块内使用 ``默认修饰词``

#### 4. fileprivate

   used within the file

  在文件内可用

#### 5. private

   used only in the enclosing declaration

   只在代码块内可用

### open vs public

1. Classes with public access, or any more restrictive access level, can be subclassed only within the module where they’re defined.

  public或其他限制的访问控制修饰的类,只能在模块内子类化
  
2. Class members with public access, or any more restrictive access level, can be overridden by subclasses only within the module where they’re defined.

   public修饰的类的成员(或其他限制的访问控制修饰的类),只能在模块内被重写

3. Open classes can be subclassed within the module where they’re defined, and within any module that imports the module where they’re defined.

   open修饰的class,可以在任何import对应模块的地方被子类化

4. Open class members can be overridden by subclasses within the module where they’re defined, and within any module that imports the module where they’re defined.

   open class 成员可以在任何import对应模块的地方被子类重写

## Custom Types (自定义类型)

#### 修饰class时 member默认的类型

open / ***public*** / internal / fileprivate / private 

open / ***internal*** / internal / fileprivate / private.
   
``只有public修饰的class的成员默认为internal，其他均为对应的类型。``

## Tuple Types (元组类型)

The access level for a tuple type is the most restrictive access level of all types used in that tuple. For example, if you compose a tuple from two different types, one with internal access and one with private access, the access level for that compound tuple type will be private.

元组的访问等级是最具限制性的。例如，如果你用两种不同的类型组成一个元组，一个是``internal``一个是``private``，元组的访问等级将成为private。

```
NOTE

Tuple types do not have a standalone definition in the way that classes, structures, enumerations, and functions do. A tuple type’s access level is deduced automatically when the tuple type is used, and cannot be specified explicitly.

与具有独立定义的类、结构体、枚举不同，元组的类型是自动推断出来的，不能被指定。
```

## Function Types (函数类型)

The access level for a function type is calculated as the most restrictive access level of the function’s parameter types and return type. You must specify the access level explicitly as part of the function’s definition if the function’s calculated access level does not match the contextual default.

函数类型的访问等级是根据函数参数和返回值的最低等级计算的。如果函数的计算等级与上下文不匹配，你必须明确指定函数类型。

## Getters and Setters

Getters and setters for constants, variables, properties, and subscripts automatically receive the same access level as the constant, variable, property, or subscript they belong to.

getters 和 setters方法访问等级与其所属的常量、变量、属性和下标一致。

You can give a setter a lower access level than its corresponding getter, to restrict the read-write scope of that variable, property, or subscript. You assign a lower access level by writing fileprivate(set), private(set), or internal(set) before the var or subscript introducer.

在变量前使用fileprivate(set), private(set), or internal(set)来实现更低级别的访问控制，以实现读写控制。

```
struct TrackedString {
    private(set) var numberOfEdits = 0
    var value: String = "" {
        didSet {
            numberOfEdits += 1
        }
    }
}
```

Note that you can assign an explicit access level for both a getter and a setter if required. The example below shows a version of the TrackedString structure in which the structure is defined with an explicit access level of public. The structure’s members (including the numberOfEdits property) therefore have an internal access level by default. You can make the structure’s numberOfEdits property getter public, and its property setter private, by combining the public and private(set) access-level modifiers:

如果需要，你可以为getter和setter同时指定一个访问级别。

```
public struct TrackedString {
    public private(set) var numberOfEdits = 0
    public var value: String = "" {
        didSet {
            numberOfEdits += 1
        }
    }
    public init() {}
}
```
