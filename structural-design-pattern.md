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
		//ctoDirectory.showEmployeeDetails();
	}
}
```
