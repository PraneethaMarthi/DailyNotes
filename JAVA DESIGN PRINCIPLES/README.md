# Solid Design Principles

* **S**ingleResponsibility Principle
* **O**pen Closed Principle
* **L**iskov Substitution Principle
* **I**nterface Segregation Principle
* **D**ependancy Inversion Principle

## Single Responsibility Principle

* There should never be more than one reason for a class to change. 
* It is focused, Single functinality adresses the specific reason.

* For Example:

```mermaid
sequenceDiagram
    participant Class
    participant RemoteServer
    Class->>RemoteServer: Protocol Change
    Class->>RemoteServer: Message Format Change
    Class->>RemoteServer: Communication Security Change
```
- From the above diagram there are three possibilty changes occuring when sending a message to remote server 

**SRPExample** 

```mermaid
classDiagram
    class main {
        +CreateUser(): boolean
    }

    class UserController{
        +ValidateUser(): Boolean
    }

    class User {
        +name: string
        +email: string
        +address: string
        +getname(): string
        +setname(): string
    }
     class store{
        +getname(): string
        +store(): void
    }
    main <|-- UserController
    User <|-- store
    User <|-- UserController
    store <|-- UserController
```

From the above example 
* If the validate methods changes in the main application then uservalidate method has to chnage in usercontroller
* if the store is changed like if the database has changed to nosql, or any other database the store method has to change
* So avoid that **Refactor** process has introduced 

**Example**

```mermaid
classDiagram
    class main {
        +CreateUser(): boolean
    }

    class UserController{
        +ValidateUser(): Boolean
    }
    class UserValidator{
        +isValidUser: boolean
        +isPresent(): boolean
        +isValidAlphaNumeric(): boolean
        +isValidEmail(): boolean
    }
    class User {
        +name: string
        +email: string
        +address: string
        +getname(): string
        +setname(): string
    }
    class UserPersistenceService{
        +saveUser():void
    }
     class store{
        +getname(): string
        +store(): void
    }
    main <|-- UserController
    UserValidator <|-- UserController
    UserPersistenceService <|-- UserController
    User <|-- UserValidator
    User <|-- store
    store <|-- UserPersistenceService
    User <|-- UserController
    store <|-- UserController
```

* So, Introducing two new classes validate user and user persistent service solves the issue.

## Open-Closed Principle

 * It states that software entities(classes,modules,methods) should be open for extension and closed for modification.
  **Open for extension** - Extend existing behaviour.
  **Closed for modification** - Existing code remains unchanged 

```mermaid
classDiagram
    class base{
                    -----closed for modification(avoid modifying base class)
    }
    class derived{
                    -----Open for extension(can derive from base and override methods)
    }
    base <|-- derived
```

**Example**

```mermaid
classDiagram

    class PhoneSubscriber{
        +subscriberId
        +Address
        +phonenumber
        +baserate
        +CalculateBill(): double
        +getterandSetter()
    }
    class ISPSubscriber{
        +subscriberId
        +Address
        +phonenumber
        +baserate
        +freeusage
        +CalculateBill(): double
        +getterandSetter()
    }
    class CallHistory {
        +SubscriberId
    }
    class InternetSessionHistory{
        +SubscriberId
    }
   PhoneSubscriber --ISPSubscriber
   PhoneSubscriber *--CallHistory
   InternetSessionHistory *--ISPSubscriber

```
From the above example, Duplication will happen to the derived class so introducing inheritance methods will solve the issue.

```mermaid
classDiagram

    class PhoneSubscriber{
        @override
        +calculatebill(): double
    }
    class ISPSubscriber{
        +CalculateBill(): double
        +getterandSetter-freeusage()
    }
    class CallHistory {
        +SubscriberId
    }
    class InternetSessionHistory{
        +SubscriberId
    }
    class Subscriber{
        +subscriberId
        +Address
        +phonenumber
        +baserate 
        +getterandsettersubscriberid          
    }
    Subscriber<|-- PhoneSubscriber
    Subscriber<|--ISPSubscriber
    PhoneSubscriber *--CallHistory
   InternetSessionHistory*--ISPSubscriber

```

## Liskov Substitution Principle

* We should be able to substitute base class objects with child class objects and this should not alter behaviour/characteristics of program.
 
 ```mermaid
classDiagram

    class Base{
        base class object providing specific behaviour 
    }

    class child{

    }
    Base <|-- child
```

```mermaid
classDiagram

    class Base{
        if substituted with child class object it should not change its behaviour
    }

    class child{

    }
    Base <|-- child
```

## Interface Seggregation Principle

* Clients should not be forced to depend upon interfaces that they donot use.
* **InterfacePollution** 
    #### Signs of Interface Pollution:
    - Classes have empty method implementations.
    - Methods implementations throw UnsupportedOperationException(or similar).
    - Method implementations return null or default/dummy values.
 To break those interfaces ISP comes into the picture.

## Dependency Inversion Principle 
* **A.** High level modules(implements business modules) should not depend upon low level modules(functionality can be used anywhere) but both should depend on abstractions(a simple interface).
* **B.** Abstractions should not depend upon details. Details should depend upon abstractions.


# Design Patterns

```mermaid
classDiagram
    class DesignPatterns{
    }
    class Creational{
        - Deals with the process of creation of objects of classes
    }
    class Structural{
         Deals with how ojects and classes are composed and arranged.
    }
    class Behavioural{
        - Describes how classes and objects communicate and interact with each other.
    }
    DesignPatterns <|-- Creational
    DesignPatterns <|-- Structural
    DesignPatterns <|-- Behavioural
```

## Creational Design Patterns :
* Deals with the process of creation of objects of classes

```mermaid
    flowchart LR
    Creational --> Builder
    Creational --> SimpleFactory
    Creational --> FactoryMethod
    Creational --> Prototype
    Creational --> Singleton
    Creational --> AbstractFactory
    Creational --> ObjectPool
```

## Builder 

* **What problem builder design pattern solves?**
 - Class Constructor requires a lot of information.
    - It will make it easy to use such constructors 
    - It will help us avoid writing such constructors in first place.
**Objects that need other objects or ""parts" to construct them.**
```mermaid
classDiagram

    class address{
    +address()
    }
    class user{
    +User(name,Address address, List<Role> roles) - Need address object, a collection of role object in order to create user object
    }
    address <|-- user
```
- In such conditions we can use builder 
- we have to create the address object first and next collection and so on. we have to follow some certain steps and we can use builder here.

* We have a complex process to construct an object involving multiple steps, then builder pattern can help us.
* In builder we remove the logic related to object construction from "client"" code & abstract it in sperate classes.

## Implement a Builder

 * We start by creating a builder
    - Identify the **parts** of the product & provide methods to create those parts.
    - It should provide a method to **assemble** or build the product/object.
    - It must provide a way/method to get fully built object out. Optionally builder can keep reference to a product it has built so the same can be returned again in future.

* A director can be a seperate class or client can play the role of director.

# Builder- Example UML

```mermaid
classDiagram
    class Client{
        <<director>>
    }
    class UserDTOBuilder{
        <<abstract>>
    }
    class UserDTO{
        <<abstract.product>>
    }
    class UserRESTDTO{

    }
    class UserRESTDTOBuilder{
        
    }
    class UserWebDTOBuilder{

    }
    class UserWebDTO{
        <<product>>
    }
    UserDTOBuilder <|-- Client
    UserDTO <|-- UserDTOBuilder
    UserDTO <|-- UserWebDTO
    UserWebDTO <|-- UserWebDTOBuilder
    UserRESTDTO <|-- UserRESTDTOBuilder
    UserDTOBuilder <-- UserRESTDTOBuilder
    UserDTOBuilder <-- UserWebDTOBuilder
```
### Implementation Considerations

* Can easily create an immutable class by implementing builder as an inner static class. you'll find this type of implementation used quite frequently even if immutability is not a main concern.

### Design Considerations

* The director role is rarely implemented as seperate class, typically the consumer of the object instance or client handles the role.
* Abstract builder is also not required if product itself is not part of anu inheritance hierarchy. you can directly create concrete builder.
* If "too many constructor arguments" problem occurs, builder pattern may help and thats an indication to use it.

**Example of a builder pattern**

* The java.lang.StringBuilder class as well as various buffer classes in java.nio package like ByteBuffer, CharBuffer are often given as examples of builder pattern.

* They dont match with 100% with GoF definition.

#### Pitfalls

- A little bit complex for beginners beacuse of method chaining where builder methods return builder object itself.
- Possibility of partitially initialized object.


## Simple Factory

* What problem simple factory solves?
    - Multiple types can be instantiated and the choice is based on some simple criteria.

* Here we simply move instantiation logic to a seperate class and most commonly to a static method of this class.
* Some donot consider simple factory to be a **Design Pattern** as its a simply method that encapsulates object instantiation 

### Implementation steps

* We start by creating a seperate class for our simple factory.
    - Add a method which returns desired object instance.
        - This method is typically static and will accept some arguments to decide which class to instantiate.
        - You can also provide additional arguments which will be used to instantiate objects.

```mermaid
classDiagram
    class Client{
        <<director>>
    }
    class SimpleFactory{
        <<static>>
        + getProduct(String): Product - Role - SimpleFactory provides static method to get instance of product subclass
    }
    class Product{
        Role Product : Objects of this class & its subclasses are needed.
    }
    class ProductA{
        <<implentationclass>>
    }
    class ProductB{
        <<implentationclass>>
    }
    SimpleFactory <|-- Client
    Product <|-- SimpleFactory
    Product <-- ProductA
    Product <-- ProductB
```

### Implementation Considerations

* SF can be a method in existing class, adding a seperate class however allows other parts of your code to use SF more easily.
* SF itself dosen't need any state tracking so best to keep it as static method.
 
### Design Considerations

* SF will in turn may use other design pattern like builder to construct objects.
* In case you want to specialize your SF in sub classes, you need factory method design pattern instead.

#### Example:
* The java.text.NumberFormat class has getIntsance method, which is example of SF

### Pitfalls

* The criteria used by SF to decide which object to instantiate can get more convoluted/complex over time. If we find ourself in such situation use Factory Method Design Pattern.