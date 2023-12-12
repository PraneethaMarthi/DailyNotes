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
