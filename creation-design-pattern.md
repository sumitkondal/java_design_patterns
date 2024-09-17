### Singleton Design Pattern :  
Singleton pattern is a design pattern in which only one instance of a class is present in the Java virtual machine. A singleton class (implementing singleton pattern) has to provide a global access point to get the instance of the class.
1. **Eager initialization** (Thread Safe – In our testing we didn’t find any duplicate hashcode) e.g. 
```java
public class GFG {
  private static final GFG instance = new GFG(); 
  private GFG()
  {  }
  public static GFG getInstance(){
        return instance;
  }
}
```
2. **Lazy initialization** (With Thread Safe) e.g.
// Java code to explain double check locking
```java
public class GFG {
private static GFG instance;
private GFG()
{}
public static GFG getInstance(){
	if (instance == null) {
		//synchronized block to remove overhead
		synchronized (GFG.class) {
			if(instance==null)	{
				instance = new GFG();
			}
		}
	}
	return instance;
}
```

3. Concept of inner static classes to use for singleton (Differ from 1. Eager In., as create instance in inner class when call getInstance method)
// Java code for Bill Pugh Singleton Implementation

```java
public class GFG {
		private GFG() {
		}
		// Inner class to provide instance of class
		private static class BillPughSingleton {
			private static final GFG INSTANCE = new GFG();
		}
		public static GFG getInstance() {
			return BillPughSingleton.INSTANCE;
		}
}
```
**Reflection and Thread Safe**
```java
public enum SingletonClass {
    INSTANCE;

    public void methodName(){
        System.out.println("In Singleton Method Class");
    }
}
public class MyApp {
    public static void main(String[] args) {
        SingletonClass instance = SingletonClass.INSTANCE;
        instance.methodName();
    }
}
```

**Factory Pattern**: Factory pattern, we create object without exposing the creation logic to the client and refer to newly created object using a common interface.
e.g.-
```java
public interface Currency {
	String getSymbol();
}
public class CurrencyFactory {	
	public static Currency getFactory (String curName) {
		if(curName.equalsIgnoreCase("Rs"))
			return new Rupee();
		else if(curName.equalsIgnoreCase("SUSD"))
			return new SGDDollar();		
		else
			throw new IllegalArgumentException("No such currency");
	}
}
public class Rupee implements Currency {
	@Override
	public String getSymbol() {
		return "Rs";
	}
}
public class SGDDollar  implements Currency {
	@Override
	public String getSymbol() {
		return "SUSD";
	}
}
public class TestFactory {	
	public static void main(String[] args) {
		Currency rupee = CurrencyFactory.getFactory("Rs");
		System.out.println(rupee.getSymbol());
	}
}
```

**Abstract Factory** Pattern: Abstract Factory Pattern is a factory of factory pattern.
Rules of thumb
- Sometimes creational patterns are competitors: there are cases when either Prototype or Abstract Factory could be used profitably. At other times they are complementary: Abstract Factory might store a set of Prototypes from which to clone and return product objects, Builder can use one of the other patterns to implement which components get built. Abstract Factory, Builder, and Prototype can use Singleton in their implementation.
- 	Abstract Factory, Builder, and Prototype define a factory object that's responsible for knowing and creating the class of product objects, and make it a parameter of the system. Abstract Factory has the factory object producing objects of several classes. Builder has the factory object building a complex product incrementally using a correspondingly complex protocol. Prototype has the factory object (aka prototype) building a product by copying a prototype object.
- Abstract Factory classes are often implemented with Factory Methods, but they can also be implemented using Prototype.
- Abstract Factory can be used as an alternative to Facade to hide platform-specific classes.
- Builder focuses on constructing a complex object step by step. Abstract Factory emphasizes a family of product objects (either simple or complex). Builder returns the product as a final step, but as far as the Abstract Factory is concerned, the product gets returned immediately.
- Often, designs start out using Factory Method (less complicated, more customizable, subclasses proliferate) and evolve toward Abstract Factory, Prototype, or Builder (more flexible, more complex) as the designer discovers where more flexibility is needed.
