### Singleton Design Pattern  
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

### Factory Pattern
Factory pattern, we create object without exposing the creation logic to the client and refer to newly created object using a common interface.
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

### Abstract Factory Pattern
Abstract Factory Pattern is a factory of factory pattern.
Rules of thumb
- Sometimes creational patterns are competitors: there are cases when either Prototype or Abstract Factory could be used profitably. At other times they are complementary: Abstract Factory might store a set of Prototypes from which to clone and return product objects, Builder can use one of the other patterns to implement which components get built. Abstract Factory, Builder, and Prototype can use Singleton in their implementation.
- 	Abstract Factory, Builder, and Prototype define a factory object that's responsible for knowing and creating the class of product objects, and make it a parameter of the system. Abstract Factory has the factory object producing objects of several classes. Builder has the factory object building a complex product incrementally using a correspondingly complex protocol. Prototype has the factory object (aka prototype) building a product by copying a prototype object.
- Abstract Factory classes are often implemented with Factory Methods, but they can also be implemented using Prototype.
- Abstract Factory can be used as an alternative to Facade to hide platform-specific classes.
- Builder focuses on constructing a complex object step by step. Abstract Factory emphasizes a family of product objects (either simple or complex). Builder returns the product as a final step, but as far as the Abstract Factory is concerned, the product gets returned immediately.
- Often, designs start out using Factory Method (less complicated, more customizable, subclasses proliferate) and evolve toward Abstract Factory, Prototype, or Builder (more flexible, more complex) as the designer discovers where more flexibility is needed.
<img width="522" alt="image" src="https://github.com/user-attachments/assets/ef759d57-e746-42f9-be9a-80274df8bf34">
```java
public abstract class Computer {
    public abstract String getRAM();
    public abstract String getHDD();
    public abstract String getCPU();
    
    @Override
    public String toString() {
        return "RAM= " + this.getRAM() + ", HDD= " + this.getHDD() + ", CPU= " + this.getCPU();
    }
}

public class ServerFactory implements ComputerAbstractFactory {
    private String ram;
    private String hdd;
    private String cpu;
    
    public ServerFactory(String ram, String hdd, String cpu) {
        this.ram = ram;
        this.hdd = hdd;
        this.cpu = cpu;
    }
    
    @Override
    public Computer createComputer() {
        return new Server(ram, hdd, cpu);
    }
}
public class PCFactory implements ComputerAbstractFactory {
    private String ram;
    private String hdd;
    private String cpu;
    
    public PCFactory(String ram, String hdd, String cpu) {
        this.ram = ram;
        this.hdd = hdd;
        this.cpu = cpu;
    }
    
    @Override
    public Computer createComputer() {
        return new PC(ram, hdd, cpu);
    }
}
public interface ComputerAbstractFactory {
    public Computer createComputer();
}
public class FactoryProducer {
    public static Computer getComputer(ComputerAbstractFactory factory) {
        return factory.createComputer();
    }
}
public class Server extends Computer {
    private String ram;
    private String hdd;
    private String cpu;
    
    public Server(String ram, String hdd, String cpu) {
        this.ram = ram;
        this.hdd = hdd;
        this.cpu = cpu;
    }
    
   // Getter Setter
}

public class PC extends Computer {
    private String ram;
    private String hdd;
    private String cpu;
    
    public PC(String ram, String hdd, String cpu) {
        this.ram = ram;
        this.hdd = hdd;
        this.cpu = cpu;
    }
    
    // Getter Setter
}
public class TestDesignPatterns {
    public static void main(String[] args) {
        // Get PC instance using PCFactory
        Computer pc = FactoryProducer.getComputer(new PCFactory("16 GB", "1 TB", "2.9 GHz"));
        System.out.println("PC Config:: " + pc);
        
        // Get Server instance using ServerFactory
        Computer server = FactoryProducer.getComputer(new ServerFactory("32 GB", "4 TB", "3.4 GHz"));
        System.out.println("Server Config:: " + server);
    }
}

```
### Prototype Pattern
Prototype pattern says that cloning of an existing object instead of creating new one and can also be customized as per the requirement. This pattern should be followed, if the cost of creating a new object is expensive and resource intensive.
Resource will save the CPU cycles but each will need memory.
In Flyweight, object is immutable.
In Prototype, object is mutable.
Prototype Steps of Example: 
1. When class load create a new instances Album/Movie class for cloning 
2. Main Method -> Prototype Test Class -> get the clone() of -> (Album/Movie class -> Override method clone() -> Call super Clone() -> Object Class Clone())
3. It will create clone copy of (Album/Movie class)

```java
public interface PrototypeCapable extends Cloneable {
	public PrototypeCapable clone() throws CloneNotSupportedException;
}

public class Movie implements PrototypeCapable {
	private String name;
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	@Override
	public Movie clone() throws CloneNotSupportedException {
		System.out.println("Cloning Movie object..");
		return (Movie) super.clone();
	}
	
}

public class Album implements PrototypeCapable {
	private String name;
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	@Override
	public Album clone() throws CloneNotSupportedException {
		System.out.println("Cloning Album object..");
		return (Album) super.clone();
	}
}
public class PrototypeFactory {
	public static PrototypeCapable getInstance(final ModelType s) throws CloneNotSupportedException {
		return (PrototypeCapable) s.getValue().clone();
	}
}
public class PrototypeFactorySimple {
	private static Map<String, PrototypeCapable> prototypes = new HashMap<String, PrototypeCapable>();
	static {
		prototypes.put("movie", new Movie());
		prototypes.put("album", new Album());
	}
	public static PrototypeCapable getInstance(final String s) throws CloneNotSupportedException {
		return ((PrototypeCapable) prototypes.get(s)).clone();
	}
}
public enum ModelType {
	MOVIE(new Movie()), ALBUM(new Album()), SHOW(new Show());
	private PrototypeCapable value;
	private ModelType(PrototypeCapable value) {
		this.value = value;
	}
	public PrototypeCapable getValue() {
		return value;
	}
}
public class TestPrototypePattern {
	public static void main(String[] args) {
		try {
			/* BEST WAY
			Movie instance1 = (Movie) PrototypeFactory.getInstance(ModelType.MOVIE);
			System.out.println(instance1.hashCode());
			Album instance2 = (Album) PrototypeFactory.getInstance(ModelType.ALBUM);
			System.out.println(instance2);
			*/
			//Alternative Simple Way
			Movie instance1 = (Movie) PrototypeFactorySimple.getInstance("movie");
			Movie instance2 = (Movie) PrototypeFactorySimple.getInstance("movie");
			System.out.println(instance1.hashCode()); //Provides a Different Hashcode value
			System.out.println(instance2.hashCode()); //Provides a Different Hashcode value
			Album instance3 = (Album) PrototypeFactorySimple.getInstance("album");
			System.out.println(instance3);
		} catch (CloneNotSupportedException e) {
			e.printStackTrace();
		}	
}
}

```
### Builder Pattern
Builder pattern says that "construct a complex object from simple objects using step-by-step approach". It is mostly used when object can't be created in single step like in the de-serialization of a complex object. 
The builder pattern helps us in creating classes with a large set of state attributes. Please note that the created object does not have any setter method, so its state cannot be changed once it has been built.
e.g. - JAVA: StringBuilder class -> .append () method 
```java
class Book {
	private final String isbn;
	private final String title;
	private final String author;
	private Book(Builder builder) {
		this.isbn = builder.isbn;
		this.title = builder.title;
		this.author = builder.author;
	}
	public String getIsbn() {
		return isbn;
	}
	public String getTitle() {
		return title;
	}
	public String getAuthor() {
		return author;
	}	
	public static class Builder {
		private final String isbn;
		private final String title;
		private String author;
		public Builder(String isbn, String title) {
			this.isbn = isbn;
			this.title = title;
		}
		public Builder author(String author) {
			this.author = author;
			return this;
		}
		public Book build() {
			return new Book(this);
		}
	}
}
public class BuilderTest {
	public static void main(String[] args) {		
		Book.Builder build = new Book.Builder("123", "Try Builder").author("ABC");	
		Book buk = build.build();
		System.out.println(buk + "|" + buk.hashCode()); //With different Hashcode
		buk = build.author("XYZ").build();
		System.out.println(buk + "|" + buk.hashCode());	//With different Hashcode
	}
}
```

