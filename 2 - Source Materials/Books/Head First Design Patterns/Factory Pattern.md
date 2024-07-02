using `new` shouldn't always be done in public and can often lead to coupling problems.

## Factory Class vs Factory Method

A static method is common and is often called `static factory`.

| Static Factory (Method) | Factory Interface |
| ---- | ---- |
| No need to instantiate object to use the create method | Can be subclassed and change the behavior of the create method |

****
## Simple Factory vs Factory Method vs Abstract Factory

| Simple Factory | Factory Method | Abstract Factory |
| ---- | ---- | ---- |
| a factory class has a method that takes an input parameter and returns a new instance of the appropriate concrete class based on that parameter | An interface that allows you to create **one** object. | An interface that allows you to create **multiple** different "objects" all related to one thing |
| Shape Factory which returns a shape | Have different classes that all generate the same object, IdGenerator class can be used with IdGeneratorFactory | Have an interface that creates widgets and buttons, you can have 3 abstract factories, one for Mac, one for Linux , and one for windows |
|  | Base Class: IdGeneratorFactory | Base Class: UserInterfaceFactory |
|  | Returns: UUIDv7IdGenerator, UUIDv6IdGenerator ... etc | Inherited: MacUIFactory, LinuxUIFactory, WindowsUIFactory. |
****
![[factory-method-vs-abstract-factory.png]]
****
