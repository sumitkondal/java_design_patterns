### Adapter Design Pattern
It allows two incompatible interfaces, classes, and services to communicate with each other. These incompatible components can talk to each other unless they change their code or the client’s behavior. It takes input from one component (client), converts it to the expected format before giving it to the other component.
We often get confused that the Adapter and the Bridge patterns are the same, however, they are not. The Adapter works on existing incompatible components, whereas the Bridge pattern is an up-front design.
e.g.-
```java
public class Volt {
    private int volts;

    public Volt(int volts) {
        this.volts = volts;
    }

    public int getVolts() {
        return volts;
    }

    public void setVolts(int volts) {
        this.volts = volts;
    }
}
```

```java
public class Socket {
    public Volt getVolt() {
        return new Volt(120);
    }
}
```
```java
public interface SocketAdapter {
    Volt get120Volt();
    Volt get12Volt();
    Volt get3Volt();
}
```
```java
public class SocketObjectAdapterImpl implements SocketAdapter {

    // Using composition for the adapter
    private Socket socket = new Socket();

    @Override
    public Volt get120Volt() {
        return socket.getVolt();
    }

    @Override
    public Volt get12Volt() {
        return convertVolt(socket.getVolt(), 10); // 120V to 12V conversion
    }

    @Override
    public Volt get3Volt() {
        return convertVolt(socket.getVolt(), 40); // 120V to 3V conversion
    }

    private Volt convertVolt(Volt v, int divisor) {
        return new Volt(v.getVolts() / divisor);
    }
}
```

```java
public class AdapterPatternTest {
    public static void main(String[] args) {
        // Testing Class Adapter
        SocketAdapter socketClassAdapter = new SocketClassAdapterImpl();
        Volt v120 = socketClassAdapter.get120Volt();
        Volt v12 = socketClassAdapter.get12Volt();
        Volt v3 = socketClassAdapter.get3Volt();
        System.out.println("Class Adapter: 120V = " + v120.getVolts() + "V, 12V = " + v12.getVolts() + "V, 3V = " + v3.getVolts() + "V");

        // Testing Object Adapter
        SocketAdapter socketObjectAdapter = new SocketObjectAdapterImpl();
        v120 = socketObjectAdapter.get120Volt();
        v12 = socketObjectAdapter.get12Volt();
        v3 = socketObjectAdapter.get3Volt();
        System.out.println("Object Adapter: 120V = " + v120.getVolts() + "V, 12V = " + v12.getVolts() + "V, 3V = " + v3.getVolts() + "V");
    }
}
```

### Bridge pattern
"Bridge pattern is used to decouple an abstraction from its implementation so that the two can vary independently".
The bridge uses encapsulation, aggregation, and can use inheritance to separate responsibilities into different classes.

Without Bridge Pattern:			             With Bridge – between Vehicle and Workshop:			            

<img width="215" alt="image" src="https://github.com/user-attachments/assets/febfbb9a-5b00-4aac-a435-f87cc789ab85">       <img width="287" alt="image" src="https://github.com/user-attachments/assets/61d33596-8dc0-480b-b54d-18a052d0d753">

Example of Vehicle
```java
public interface Workshop {
    void work();
}
public class Produce implements Workshop {
    @Override
    public void work() {
        System.out.println("Producing parts");
    }
}

public class Assemble implements Workshop {
    @Override
    public void work() {
        System.out.println("Assembling vehicle");
    }
}

public abstract class Vehicle {
    protected Workshop workshop;

    public Vehicle(Workshop workshop) {
        this.workshop = workshop;
    }

    abstract public void manufacture();
}
public class Bus extends Vehicle {
    public Bus(Workshop workshop) {
        super(workshop);
    }

    @Override
    public void manufacture() {
        System.out.print("Bus ");
        workshop.work();
    }
}

public class Bike extends Vehicle {
    public Bike(Workshop workshop) {
        super(workshop);
    }

    @Override
    public void manufacture() {
        System.out.print("Bike ");
        workshop.work();
    }
}
public class BridgePatternDemo {
    public static void main(String[] args) {
        Vehicle vehicle1 = new Bus(new Produce());
        vehicle1.manufacture();

        Vehicle vehicle2 = new Bus(new Assemble());
        vehicle2.manufacture();

        Vehicle vehicle3 = new Bike(new Produce());
        vehicle3.manufacture();

        Vehicle vehicle4 = new Bike(new Assemble());
        vehicle4.manufacture();
    }
}
```
Example on Pen color
<img width="287" alt="image" src="https://github.com/user-attachments/assets/6364a9bd-5c5a-47ab-981d-82774a8a91be">

```java
interface Color {
	String fill();
}
class Blue implements Color {
	@Override
	public String fill() {
		return "Color is Blue";
	}
}
class Red implements Color {
	@Override
	public String fill() {
		return "Color is Red";
	}
}
abstract class Shape {
	protected Color color;
	public Shape(Color color) {
		super();
		this.color = color;
	}
	public abstract String draw();
}
class Square extends Shape {
	public Square(Color color) {
		super(color);
	}
	@Override
	public String draw() {
		return "Square drawn. " + color.fill();
	}
}
class Circle extends Shape {
	public Circle(Color color) {
		super(color);
	}
	@Override
	public String draw() {
		return "Circle drawn. " + color.fill();
	}
}
public class BridgePattern1 {
	public static void main(String[] args) {
		Shape square = new Square(new Red());
		System.out.println(square.draw());
		System.out.println(new Square(new Blue()).draw());
	}
}
```

### Composite Design Pattern
Composite design pattern compose objects into tree structures to represent whole-part hierarchies. Composite lets clients treat individual objects and compositions of objects uniformly. 
It treats each node in two ways:
1) Composite – Composite means it can have other objects below it.
2) Leaf – leaf means it has no objects below it.

<img width="132" alt="image" src="https://github.com/user-attachments/assets/099a3261-0fc4-478c-b7a0-185f2a0fa00b">

```java
interface Employee{
	public void showEmployeeDetails();
}
```

```java
class Employee_Leaf implements Employee{
	private int dev_id;
	private String dev_name;
	private String team_name;
	public Employee_Leaf(int dev_id, String dev_name, String team_name) {
		this.dev_id = dev_id;
		this.dev_name = dev_name;
		this.team_name = team_name;
	}
	public String getTeam_name() {
		return team_name;
	}
	public void setTeam_name(String team_name) {
		this.team_name = team_name;
	}
	@Override
	public void showEmployeeDetails() {
		System.out.println("Dev |"+dev_id+" | "+dev_name+" | "+team_name);
	}
	public int getDev_id() {
		return dev_id;
	}
	public void setDev_id(int dev_id) {
		this.dev_id = dev_id;
	}
	public String getDev_name() {
		return dev_name;
	}
	public void setDev_name(String dev_name) {
		this.dev_name = dev_name;
	}
}
```
```java
class CompanyDirectory implements Employee{
	private String deptName;
	public CompanyDirectory(String deptName) {
		this.deptName = deptName;
	}
	private List<Employee> employees=new ArrayList<Employee>();
	@Override
	public void showEmployeeDetails() {
		System.out.println(deptName);
		for (Employee e:employees) {
			e.showEmployeeDetails();
		}
	}
	public void addEmployee(Employee e) {
		employees.add(e);
	}
	public void removeEmployee(Employee e) {
		employees.remove(e);
	}
}
```
```java
public class TestComposite {
	public static void main(String[] args) {
		Employee dev1 = new Employee_Leaf(100, "Sumit", "BACKEND");
		Employee dev2 = new Employee_Leaf(101, "Neeraj", "BACKEND");
		
		Employee dev3 = new Employee_Leaf(103, "Ashish", "FRONTEND");
		Employee dev4 = new Employee_Leaf(104, "Ravi", "FRONTEND");
		
		Employee dev5 = new Employee_Leaf(105, "Mahesh","HR");
		Employee dev6 = new Employee_Leaf(106, "Ankit","HR");
		
		Employee dev7 = new Employee_Leaf(107, "Mohit","SALES");
		Employee dev8 = new Employee_Leaf(108, "Ankur","SALES");
		
		CompanyDirectory tbvpDirectory = new CompanyDirectory("BACKEND");
		CompanyDirectory tfvpDirectory = new CompanyDirectory("FRONTEND");
		CompanyDirectory hrvpDirectory = new CompanyDirectory("HR");
		CompanyDirectory svpDirectory = new CompanyDirectory("SALES");
		CompanyDirectory ctoDirectory = new CompanyDirectory("CTO");
		CompanyDirectory cfoDirectory = new CompanyDirectory("CFO");
		CompanyDirectory ceoDirectory = new CompanyDirectory("CEO");
		
		tbvpDirectory.addEmployee(dev1);
		tbvpDirectory.addEmployee(dev2);
		
		tfvpDirectory.addEmployee(dev3);
		tfvpDirectory.addEmployee(dev4);
		
		hrvpDirectory.addEmployee(dev5);
		hrvpDirectory.addEmployee(dev6);
		
		svpDirectory.addEmployee(dev7);
		svpDirectory.addEmployee(dev8);
		
		ctoDirectory.addEmployee(tbvpDirectory);
		ctoDirectory.addEmployee(tfvpDirectory);
		
		cfoDirectory.addEmployee(hrvpDirectory);
		cfoDirectory.addEmployee(svpDirectory);
		
		ceoDirectory.addEmployee(ctoDirectory);
		ceoDirectory.addEmployee(cfoDirectory);
	
		ceoDirectory.showEmployeeDetails();
		ctoDirectory.showEmployeeDetails();
	}
}
```

### Decorator Pattern
The decorator design pattern is used to change an object’s functionality during runtime. Other instances of the same class will be unaffected, therefore each object will have its behavior changed.

```java
interface Icecream {
	public String makeIcecream();
}
class SimpleIcecream implements Icecream {
	@Override
	public String makeIcecream() {
		return "Base Icecream";
	}
}
class IcecreamDecorator implements Icecream {
    protected Icecream specialIcecream;
    public IcecreamDecorator(Icecream specialIcecream) {
        this.specialIcecream = specialIcecream;
    }
    public String makeIcecream() {
        return specialIcecream.makeIcecream();
    }
}
class NuttyDecorator extends IcecreamDecorator {
    public NuttyDecorator(Icecream specialIcecream) {
        super(specialIcecream);
    }
    public String makeIcecream() {
        return specialIcecream.makeIcecream() + addNuts();
    }
    private String addNuts() {
        return " + crunchy nuts";
    }
}
class HoneyDecorator extends IcecreamDecorator {
    public HoneyDecorator(Icecream specialIcecream) {
        super(specialIcecream);
    }
    public String makeIcecream() {
        return specialIcecream.makeIcecream() + addHoney();
    }
    private String addHoney() {
        return " + sweet honey";
    }
}
public class TestDecorator {
    public static void main(String args[]) {
    	Icecream icecream = new NuttyDecorator(new HoneyDecorator(new SimpleIcecream()));
        System.out.println(icecream.makeIcecream());
    }
}
```

### Facade Pattern
Provide a unified interface to a set of interfaces in a subsystem. Facade Pattern defines a higher-level interface that makes the subsystem easier to use.
- It shields the clients from the complexities of the sub-system components.
- It promotes loose coupling between subsystems and its clients.
e.g.
```java
public class MySqlHelper {
	public static Connection getMySqlDBConnection() {
		return null;
	}
	public void generateMySqlPDFReport(String tableName, Connection con) {
	}
	public void generateMySqlCSVReport(String tableName, Connection con) {
	}
}
```
```java
class OracleHelper {
	public static Connection getOracleDBConnection() {
		return null;
	}
	public void generateOraclePDFReport(String tableName, Connection con) {
	}
	public void generateOracleCSVReport(String tableName, Connection con) {
	}
}
```

```java
class HelperFacade {
	public static void generateReport(String dbType, String reportType, String tableName){
		Connection con = null;
		switch (dbType) {
		case "MYSQL": 
			con = MySqlHelper.getMySqlDBConnection();
			MySqlHelper mySqlHelper = new MySqlHelper();
			switch(reportType){
			case "CSV":
				mySqlHelper.generateMySqlCSVReport(tableName, con);
				break;
			case "PDF":
				mySqlHelper.generateMySqlPDFReport(tableName, con);
				break;
			}
			break;
		case "ORACLE": 
			con = OracleHelper.getOracleDBConnection();
			OracleHelper oracleHelper = new OracleHelper();
			switch(reportType){
			case "CSV":
				oracleHelper.generateOracleCSVReport(tableName, con);
				break;
			case "PDF":
				oracleHelper.generateOraclePDFReport(tableName, con);
				break;
			}
			break;
		}
	}
}
```
```java
public class FacadePatternTest {
	public static void main(String[] args) {
		String tableName="Employee";
		
		//generating MySql HTML report and Oracle PDF report without using Facade
		Connection con = MySqlHelper.getMySqlDBConnection();
		MySqlHelper mySqlHelper = new MySqlHelper();
		mySqlHelper.generateMySqlCSVReport(tableName, con);
		
		Connection con1 = OracleHelper.getOracleDBConnection();
		OracleHelper oracleHelper = new OracleHelper();
		oracleHelper.generateOraclePDFReport(tableName, con1);
		
		//generating MySql HTML report and Oracle PDF report using Facade
		HelperFacade.generateReport("MYSQL", "CSV", tableName);
		HelperFacade.generateReport("ORACLE", "PDF", tableName);
	}
}
```

### Flyweight Pattern
it is used when we need to create a lot of Objects of a class. A flyweight is a shared object that can be used in multiple contexts simultaneously. The flyweight acts as an independent object in each context.
To reuse already existing similar kind of objects by storing them and create new object when no matching object is found.

A flyweight objects essentially has two kind of attributes – intrinsic and extrinsic.
An intrinsic state attribute is stored/shared in the flyweight object, and it is independent of flyweight’s context. As the best practice, we should make intrinsic states immutable.

**Advantage**
- 	It reduces the number of objects.
- 	It reduces the amount of memory and storage devices required if the objects are persisted.

**Usage**
- When an application uses number of objects
- 	When the storage cost is high because of the quantity of objects.
- 	When the application does not depend on object identity.


```java
interface Pen {
	public void draw(String content);
}

class ThickPen implements Pen {
	private String color = null; // extrinsic state - supplied by client

	public ThickPen(String color) {
		this.color = color;
	}
	@Override
	public void draw(String content) {
		System.out.println(content + "Drawing THICK content in color : " + color);
	}
}

class ThinPen implements Pen {
	private String color = null;

	public ThinPen(String color) {
		this.color = color;
	}
	@Override
	public void draw(String content) {
		System.out.println("Drawing THIN content in color : " + color);
	}
}
```

```java
class PenFactory {
	private static final Map<String, Pen> pensMap = new HashMap<>();

	public static Pen getThickPen(String color) {
		String key = color + "-THICK";
		Pen pen = pensMap.get(key);
		if (pen != null) {
			return pen;
		} else {
			pen = new ThickPen(color);
			pensMap.put(key, pen);
		}
		return pen;
	}
	public static Pen getThinPen(String color) {
		String key = color + "-THIN";
		Pen pen = pensMap.get(key);
		if (pen != null) {
			return pen;
		} else {
			pen = new ThinPen(color);
			pensMap.put(key, pen);
		}

		return pen;
	}
}
```
```java
public class FlyWeightEx {
	public static void main(String[] args) {
		Pen yellowThinPen1 = PenFactory.getThickPen("YELLOW"); // created new pen
		yellowThinPen1.draw("Hello World !!");

		Pen yellowThinPen2 = PenFactory.getThickPen("YELLOW"); // pen is shared
		yellowThinPen2.draw("Hello World !!");

		Pen blueThinPen = PenFactory.getThickPen("BLUE"); // created new pen
		blueThinPen.draw("Hello World !!");

		System.out.println(yellowThinPen1.hashCode()); //output : 366712642
		System.out.println(yellowThinPen2.hashCode()); //366712642

		System.out.println(blueThinPen.hashCode()); //1829164700
	}
}

```

### Proxy Pattern
A proxy object hides the original object and control access to it. We can use proxy when we may want to use a class that can perform as an interface to something else.
Proxy is heavily used to implement lazy loading related usecases where we do not want to create full object until it is actually needed.
A proxy can be used to add an additional security layer around the original object as well.

In Real World use in: Hibernate JPA, Spring AOP.

```java
interface RealObject {
	public void doSomething();
}
class RealObjectImpl implements RealObject {
	@Override
	public void doSomething() {
		System.out.println("Performing work in real object");
	}
}
class RealObjectProxy extends RealObjectImpl {
	@Override
	public void doSomething() {
		// Perform additional logic and security
		// Even we can block the operation execution
		System.out.println("Delegating work on real object");
		super.doSomething();
	}
}
public class Client {
	public static void main(String[] args) {
		RealObject proxy = new RealObjectProxy();
		proxy.doSomething();
	}
}
```
