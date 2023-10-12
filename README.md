# java_design_patterns
Solutions to the common problem in software development a Design patterns

### Creational (Focus on object creation mechanisms, providing flexible ways to create instances of classes.) - SFAPB
* **Singleton**: Ensures that there is only one instance of a class. Use case: Logging, Configuration, Database, and Cache.
* **Factory**: Create object without exposing the creation logic to client. Defines interface for creating objects, leave implementation up to subclasses. 
* **Abstract Factory**: Provides a way to create families of related objects without specifying their concrete classes.
* **Prototype**: Creates a new object by copying an existing object. (Cloning of an existing object instead of creating new one to save resources)
* **Builder**: Construct a complex object from simple objects using step-by-step approach. (Helps in creating classes with a large set of state attributes)
### Structural (Focus on the composition of classes and objects to form larger structures) - ABCD-FFP
* **Adapter**: Allows two incompatible interfaces, classes, and services to communicate and work together.
* **Bridge**: Separates the abstraction from its implementation. Useful when you want to change implementations without affecting clients.
* **Composite**: Compose objects into tree structures to represent whole-part hierarchies. - Composite (having children), Leaf (no children)
* **Decorator**: Dynamically adds new functionality to object. Change object’s functionality during runtime. Extending functionality without subclassing.
* **Facade**: Provides a simplified interface to a complex subsystem.
* **Flyweight**: Minimizes memory usage by sharing data across multiple objects, when many instances have similar states. It reduces the no. of objects
* **Proxy**: Used to add an extra security layer around the original object as well. It hides the original object. (Lazy loading, access control, or logging)
### Behavioral (How objects interact and communicate with each other to achieve certain behaviors and responsibilities) - CCII-MMOS-STV
* **Chain of Responsibility**: Allows you to pass a request along a chain of objects until an object is found that can handle the request.
* **Command**: Encapsulate actions as objects, making it easy to queue, log, or undo them without knowing all the details of how they're executed. 
* **Interpreter**: Provides a way to interpret language grammar or expressions. Useful for creating domain-specific languages.
* **Iterator**: Provides a way to traverse elements of a collection sequentially without exposing its underlying representation.
* **Mediator**: Reduces coupling between objects by providing a single object to manage their interactions and reducing dependencies.
* **Memento**: Allows you to create a snapshot of the state of an object, for undo/redo functionality or restoring an object to a previous state.
* **Observer**: Define a 1: M relationship btw objects so that when one obj changes state, all of its dependents are notified and updated automatically.
* **State**: Allow object for changing its behavior without changing its class. Cleaner without many if/else statement. (Single Responsibility Principle and Open/Closed Principle from SOLID principles)
* **Strategy**: Family of algorithms, encapsulates them, and makes them interchangeable. Clients can choose the desired algorithm at runtime.
* **Template**: An algorithm as a skeleton of operations and leave the details to be implemented by the child classes. Sequence preserved by parent.
* **Visitor**: Separates an algorithm from an object's structure, allowing new operations to be added without modifying the objects.
