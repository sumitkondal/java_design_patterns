### Chain of Responsibility Pattern
Chain of Responsibility as a design pattern consisting of 
“A source of command objects and a series of processing objects”.
Each processing object in the chain is responsible for a certain type of command, and the processing is done, it forwards the command to the next processor in the chain.
- Decoupling a sender and receiver of a command
- Picking a processing strategy at processing-time
Use in: ATM, Vending Machine Design

```java
interface DispenseChain {
	void setNextChain(DispenseChain nextChain);
	void dispense(Currency cur);
}

class Currency {
	private int amount;
	public Currency(int amt) {
		this.amount = amt;
	}
	public int getAmount() {
		return this.amount;
	}
}
```
```java
class Dollar50Dispenser implements DispenseChain {
	private DispenseChain chain;
	@Override
	public void setNextChain(DispenseChain nextChain) {
		this.chain = nextChain;
	}
	@Override
	public void dispense(Currency cur) {
		if (cur.getAmount() >= 50) {
			int num = cur.getAmount() / 50;
			int remainder = cur.getAmount() % 50;
			System.out.println("Dispensing " + num + " 50$ note");
			if (remainder != 0)
				this.chain.dispense(new Currency(remainder));
		} else {
			this.chain.dispense(cur);
		}
	}
}
```
```java
class Dollar20Dispenser implements DispenseChain {
	private DispenseChain chain;
	@Override
	public void setNextChain(DispenseChain nextChain) {
		this.chain = nextChain;
	}
	@Override
	public void dispense(Currency cur) {
		if (cur.getAmount() >= 20) {
			int num = cur.getAmount() / 20;
			int remainder = cur.getAmount() % 20;
			System.out.println("Dispensing " + num + " 20$ note");
			if (remainder != 0)
				this.chain.dispense(new Currency(remainder));
		} else {
			this.chain.dispense(cur);
		}
	}
}
```

```java
class Dollar10Dispenser implements DispenseChain {
	private DispenseChain chain;
	@Override
	public void setNextChain(DispenseChain nextChain) {
		this.chain = nextChain;
	}
	@Override
	public void dispense(Currency cur) {
		if (cur.getAmount() >= 10) {
			int num = cur.getAmount() / 10;
			int remainder = cur.getAmount() % 10;
			System.out.println("Dispensing " + num + " 10$ note");
			if (remainder != 0)
				this.chain.dispense(new Currency(remainder));
		} else {
			this.chain.dispense(cur);
		}
	}
}
```
```java
public class ATMDispenseChain {
		private DispenseChain c1;
		public ATMDispenseChain() {
			// initialize the chain
			this.c1 = new Dollar50Dispenser();
			DispenseChain c2 = new Dollar20Dispenser();
			DispenseChain c3 = new Dollar10Dispenser();
			// set the chain of responsibility
			c1.setNextChain(c2);
			c2.setNextChain(c3);
		}
		public static void main(String[] args) {
			ATMDispenseChain atmDispenser = new ATMDispenseChain();
			int amount = 80;				
			// process the request
			atmDispenser.c1.dispense(new Currency(amount));
		}
}
```
### Command Pattern
A Command Pattern says that "encapsulate a request under an object as a command and pass it to invoker object. Invoker object looks for the appropriate object which can handle this command and pass the command to the corresponding object and that object executes the command".

It is also known as Action or Transaction.

```java
interface ICommand {
	public void execute();
}
class Light {
	String name;	
	public Light(String name) {
		this.name = name;
	}
	public void turnOn() {
		System.out.println(name + " Light is on" );
	}
	public void turnOff() {
		System.out.println( name + " Light is off");
	}
}
class Fan {
	void start() {
		System.out.println("Fan Started..");
	}
	void stop() {
		System.out.println("Fan stopped..");
	}
}
```
```java
class TurnOffLightCommand implements ICommand {
	Light light;
	public TurnOffLightCommand(Light light) {
		this.light = light;
	}
	@Override
	public void execute() {
		System.out.println("Turning off light.");
		light.turnOff();
	}
}
class TurnOnLightCommand implements ICommand {
	Light light;
	public TurnOnLightCommand(Light light) {
		this.light = light;
	}
	@Override
	public void execute() {
		System.out.println("Turning on light.");
		light.turnOn();
	}
}
```
```java
class StartFanCommand implements ICommand {
	Fan fan;
	public StartFanCommand(Fan fan) {
		this.fan = fan;
	}
	@Override
	public void execute() {
		System.out.println("starting Fan.");
		fan.start();
	}
}
class StopFanCommand implements ICommand {
	Fan fan;
	public StopFanCommand(Fan fan) {
		this.fan = fan;
	}
	@Override
	public void execute() {
		System.out.println("stopping Fan.");
		fan.stop();
	}
}
```
```java
class HomeAutomationRemote {
	ICommand command;
	public void setCommand(ICommand command) {
		this.command = command;
	}
	// Will call the command interface method so that particular command can be
	// invoked.
	public void buttonPressed() {
		command.execute();
	}
}
```
```java
public class CommandPatternDemo // client
{
	public static void main(String[] args) {
		Light livingRoomLight = new Light("Living Room"); // receiver 1
		Fan livingRoomFan = new Fan(); // receiver 2
		Light bedRoomLight = new Light("Bed Room"); // receiver 3
		Fan bedRoomFan = new Fan(); // receiver 4

		HomeAutomationRemote remote = new HomeAutomationRemote(); // Invoker

		remote.setCommand(new TurnOnLightCommand(livingRoomLight));
		remote.buttonPressed();
		remote.setCommand(new TurnOnLightCommand(bedRoomLight));
		remote.buttonPressed();
		remote.setCommand(new StartFanCommand(livingRoomFan));
		remote.buttonPressed();
		remote.setCommand(new StopFanCommand(livingRoomFan));
		remote.buttonPressed();
		remote.setCommand(new StartFanCommand(bedRoomFan));
		remote.buttonPressed();
		remote.setCommand(new StopFanCommand(bedRoomFan));
		remote.buttonPressed();
	}
}
```

### Iterator Pattern
The Iterator Pattern is a basic and widely used design pattern. Each language has a large number of data collections as well as structures. Iterator is known to be a behavioral design pattern that allows you to traverse components of a set without revealing the representation underneath (for example: stack, lists, tree, etc.).

The participants of iterator pattern are as follows:
- Iterator: An interface to access or traverse the elements collection. Provide methods which concrete iterators must implement.
- Concrete Iterator: implements the Iterator interface methods. It can also keep track of the current position in the traversal of the aggregate collection.
- Aggregate: It is typically a collection interface which defines a method that can create an Iterator object.
- Concrete Aggregate: It implements the Aggregate interface and its specific method returns an instance of Concrete Iterator.

```java
interface Iterator<E> {
	void reset(); // reset to the first element
	E next(); // To get the next element
	E currentItem(); // To retrieve the current element
	boolean hasNext(); // To check whether there is any next element or not.
}
```

```java
class Topic {
	private String name;
	public Topic(String name) {
		this.name = name;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
}

class TopicIterator implements Iterator<Topic> {
	private Topic[] topics;
	private int position;
	public TopicIterator(Topic[] topics) {
		this.topics = topics;
		position = 0;
	}
	@Override
	public void reset() {
		position = 0;
	}
	@Override
	public Topic next() {
		return topics[position++];
	}
	@Override
	public Topic currentItem() {
		return topics[position];
	}
	@Override
	public boolean hasNext() {
		if (position >= topics.length)
			return false;
		return true;
	}
}
```
```java
interface List<E> {
	Iterator<E> iterator();
}
```
```java
class TopicList implements List<Topic> {
	private Topic[] topics;

	public TopicList(Topic[] topics) {
		this.topics = topics;
	}

	@Override
	public Iterator<Topic> iterator() {
		return new TopicIterator(topics);
	}
}
```
```java
public class IteratorPatternDemo {
	public static void main(String[] args) {
		Topic[] topics = new Topic[5];
		topics[0] = new Topic("topic1");
		topics[1] = new Topic("topic2");
		topics[2] = new Topic("topic3");
		topics[3] = new Topic("topic4");
		topics[4] = new Topic("topic5");
		List<Topic> list = new TopicList(topics);
		Iterator<Topic> iterator = list.iterator();
		while (iterator.hasNext()) {
			Topic currentTopic = iterator.next();
			System.out.println(currentTopic.getName());
		}
	}
}
```
### Mediator Pattern
**1** - We should always try to design the system in such a way that components are loosely coupled and reusable. This approach makes our code easier to maintain and test.
In real life, however, we often need to deal with a complex set of dependent objects. This is when the Mediator Pattern may come in handy.
The intent of the Mediator Pattern is to reduce the complexity and dependencies between tightly coupled objects communicating directly with one another.

**2** - Mediator helps in establishing loosely coupled communication between objects and helps in reducing the direct references to each other. This helps in minimizing the complexity of dependency management and communications among participating objects.
**Main Points:**
- Chain of Responsibility passes a request sequentially along a dynamic chain of potential receivers until one of them handles it.
- Command establishes unidirectional connections between senders and receivers.
- Mediator eliminates direct connections between senders and receivers, forcing them to communicate indirectly via a mediator object.
- Observer lets receivers dynamically subscribe to and unsubscribe from receiving requests.

- Facade and Mediator have similar jobs: they try to organize collaboration between lots of tightly coupled classes.

	- Facade defines a simplified interface to a subsystem of objects, but it doesn’t introduce any new functionality. The subsystem itself is unaware of the facade. Objects within the subsystem can communicate directly.

	- Mediator centralizes communication between components of the system. The components only know about the mediator object and don’t communicate directly.

**Wrong Practice (Tight Coupled)**
e.g. – 
```java
class Button {
	private Fan fan;	
	// constructor, getters and setters
	public void press() {
		if (fan.isOn()) {
			fan.turnOff();
		} else {
			fan.turnOn();
		}
	}
}
```
```java
class Fan {
	private Button button;
	private PowerSupplier powerSupplier;
	private boolean isOn = false;
	// constructor, getters and setters
	public boolean isOn() {
		return isOn;
	}
	public void turnOn() {
		powerSupplier.turnOn();
		isOn = true;
	}	
	public void turnOff() {
		isOn = false;
		powerSupplier.turnOff();
	}
}
```
```java
class PowerSupplier {
	public void turnOn() {
		System.out.println("Power Supply On");
	}
	public void turnOff() {
		System.out.println("Power Supply Off");
	}
}
public class JavaThreadsInterviewQuestions {
	public static void main(String[] args) {
		Button btn1 = new Button();
		PowerSupplier pwrSupply = new PowerSupplier();
		Fan fn = new Fan();
		fn.setPowerSupplier(pwrSupply);
		btn1.setFan(fn);

		btn1.press();
		btn1.press();
	}
}
```
**Correct :**

```java
class Button {
	private Mediator mediator;
	// constructor, getters and setters
public void setMediator(Mediator mediator) {
        		this.mediator = mediator;
   	}
	public void press() {
		mediator.press();
	}
}
```
```java
class Fan {
	private Mediator mediator;
	private boolean isOn = false;
	// constructor, getters and setters
public void setMediator(Mediator mediator) {
       this.mediator = mediator;
   	}
	public boolean isOn() {
		return isOn;
	}
	public void turnOn() {
		mediator.start();
		isOn = true;
	}
	public void turnOff() {
		isOn = false;
		mediator.stop();
	}
}
```
```java
class PowerSupplier {
	public void turnOn() {
		System.out.println("Power Supply On");
	}
	public void turnOff() {
		System.out.println("Power Supply Off");
	}
}
```
```java
class Mediator {
	private Button button;
	private Fan fan;
	private PowerSupplier powerSupplier;
	// constructor, getters and setters
public void setButton(Button button) {
        this.button = button;
        this.button.setMediator(this);
}
public void setFan(Fan fan) {
        this.fan = fan;
        this.fan.setMediator(this);
}
public void setPowerSupplier(PowerSupplier powerSupplier) {
        this.powerSupplier = powerSupplier;
}
	public void press() {
		if (fan.isOn()) {
			fan.turnOff();
		} else {
			fan.turnOn();
		}
	}
	public void start() {
		powerSupplier.turnOn();
	}
	public void stop() {
		powerSupplier.turnOff();
	}
}
```
```java
public class JavaThreadsInterviewQuestions {
	public static void main(String[] args) {
		Button btn1 = new Button();
		PowerSupplier pwrSupply = new PowerSupplier();
		Fan fn = new Fan();
		Mediator mdtr = new Mediator();
		
		mdtr.setButton(btn1);
		mdtr.setFan(fn);
		mdtr.setPowerSupplier(pwrSupply);
		
		btn1.press();
		btn1.press();	
	}
}
```
### Observer Pattern
Observer is a behavioral design pattern. It specifies communication between objects: observable and observers. An observable is an object which notifies observers about the changes in its state.
It can be implemented by
- Implement By Own
- Implementation with Observer
- Implementation with PropertyChangeListener

Deprecated from java 9 – Class java.util.**Observable**
addObserver(Observer o)
deleteObserver(Observer o)

Interface **Observer**
update(Observable o, Object arg)

Now can use Reactive Streams or Flow API(Package java.util.concurrent) –
Flow.Processor : A component that acts as both a Subscriber and Publisher.
Flow.Publisher : A producer of items received by Subscribers.
Flow.Subscriber : A receiver of messages.
Flow.Subscription: Message control linking a Flow.Publisher and Flow.Subscriber.

e.g.
```java
class Observer1 implements Observer
{
	@Override
	public void update(Observable obj, Object arg)
	{
		System.out.println("Observer1 - " + arg);
	}
}
```
```java
class Observer2 implements Observer
{
	@Override
	public void update(Observable obj, Object arg)
	{
		System.out.println("Observer2 - " + arg);
	}
}
```
```java
class BeingObserved extends Observable
{
	void incre()
	{
		setChanged();
		notifyObservers("New Item Found in your Stock, Please Buy....");
	}
}
```
```java
class ObserverDemo {
	public static void main(String args[])
	{
		BeingObserved beingObserved = new BeingObserved();
		Observer1 observer1 = new Observer1();
		Observer2 observer2 = new Observer2();
		beingObserved.addObserver(observer1);
		beingObserved.addObserver(observer2);
		beingObserved.incre();
	}
}
```

Use Case : FlipKart / Amazon Design
```java
class UserSubscriber implements Observer {
	@Override
	public void update(Observable o, Object arg) {
		System.out.println("Mail send to Subscriber, Total Stock available - " + arg);
	}
}
```
```java
class IphoneSubscriber implements Observer {
	@Override
	public void update(Observable o, Object arg) {
		System.out.println("Mail send to Iphone Subscriber, Total Stock available - " + arg);
	}
}
```
```java
class NotifyMeFk {
	public void notifyMe(NewStock stock, Observer user) {
		stock.addObserver(user);
	}
}
```
```java
class NewStock extends Observable {
	public void addOutOfStockItems(int stockCnt) {
		setChanged();
		notifyObservers(stockCnt);
	}
}
```
```java
public class FlipKartNotifyMe {
	public static void main(String[] args) {
		Observer user1 = new UserSubscriber();
		Observer user3 = new IphoneSubscriber();
		NewStock stock = new NewStock();
		NotifyMeFk notme = new NotifyMeFk();
		notme.notifyMe(stock, user1);
		notme.notifyMe(stock, user3);
		stock.addOutOfStockItems(5);
	}
}
```

### State Pattern
The main idea of State pattern is to **allow the object for changing its behavior without changing its class**. Also, by implementing it, the code should remain cleaner without many if/else statements.
State design pattern, we can encapsulate the logic in dedicated classes, apply the Single Responsibility Principle and Open/Closed Principle, have cleaner and more maintainable code.

e.g.-
```java
interface PackageState {
	void next(Package pkg);
	void prev(Package pkg);
	void checkStatus();
}
```
```java
class Package {
	private PackageState state = new OrderedState();
	public PackageState getState() {
		return state;
	}
	public void setState(PackageState state) {
		this.state = state;
	}
	public void previousState() {
		state.prev(this);
	}
	public void nextState() {
		state.next(this);
	}
	public void checkStatus() {
		state.checkStatus();
	}
}
```
```java
class OrderedState implements PackageState {
	@Override
	public void next(Package pkg) {
		pkg.setState(new DeliveredState());
	}
	@Override
	public void prev(Package pkg) {
		System.out.println("The package is in its root state.");
	}
	@Override
	public void checkStatus() {
		System.out.println("Package ordered, not delivered to the office yet.");
	}
}
```
```java
class DeliveredState implements PackageState {
	@Override
	public void next(Package pkg) {
		pkg.setState(new ReceivedState());
	}
	@Override
	public void prev(Package pkg) {
		pkg.setState(new OrderedState());
	}
	@Override
	public void checkStatus() {
		System.out.println("Package delivered to post office, not received yet.");
	}
}
```
```java
class ReceivedState implements PackageState {
	@Override
	public void next(Package pkg) {
		System.out.println("This package is already received by a client.");
	}
	@Override
	public void prev(Package pkg) {
		pkg.setState(new DeliveredState());
	}
	@Override
	public void checkStatus() {
		System.out.println("Package Received to Client.");
	}
}
```

```java
public class StateDemo {
	public static void main(String[] args) {
		Package pkg = new Package();
		pkg.checkStatus();

		pkg.nextState();
		pkg.checkStatus();

		pkg.nextState();
		pkg.checkStatus();

		pkg.nextState();
		pkg.checkStatus();
	}
}
```
### Strategy Pattern
A Strategy Pattern says that **"defines a family of functionality, encapsulate each one, and make them interchangeable"**.
The Strategy Pattern is also known as **Policy**.
e.g.-
```java
interface Strategy {
	public float calculation(float a, float b);
}
```
```java
class Addition implements Strategy {
	@Override
	public float calculation(float a, float b) {
		return a + b;
	}
}
```
```java
class Subtraction implements Strategy {
	@Override
	public float calculation(float a, float b) {
		return a - b;
	}
}
```
```java
class Multiplication implements Strategy {
	@Override
	public float calculation(float a, float b) {
		return a * b;
	}
}
```
```java
class Context {
	private Strategy strategy;
	public Context(Strategy strategy) {
		this.strategy = strategy;
	}
	public float executeStrategy(float num1, float num2) {
		return strategy.calculation(num1, num2);
	}
}
```
```java
public class StrategyPatternDemo {
	public static void main(String[] args) {
		float value1 = 4.67f;
		float value2 = 34.56f;
		Context context = new Context(new Addition());
		System.out.println("Addition = " + context.executeStrategy(value1, value2));

		context = new Context(new Subtraction());
		System.out.println("Subtraction = " + context.executeStrategy(value1, value2));

		context = new Context(new Multiplication());
		System.out.println("Multiplication = " + context.executeStrategy(value1, value2));
	}
}
```
### Template Pattern
Template method design pattern is to define **an algorithm as a skeleton of operations and leave the details to be implemented by the child classes**. The overall structure and sequence of the algorithm are preserved by the parent class.
e.g.-
```java
abstract class Game {
	abstract void initialize();
	abstract void startPlay();
	// template method
	public final void play() {
		initialize();
		startPlay();
	}
}
```
```java
class Cricket extends Game {
	@Override
	void initialize() {
		System.out.println("Cricket Game Initialized! Start playing.");
	}
	@Override
	void startPlay() {
		System.out.println("Cricket Game Started. Enjoy the game!");
	}
}
```
```java
class Football extends Game {
	@Override
	void initialize() {
		System.out.println("Football Game Initialized! Start playing.");
	}
	@Override
	void startPlay() {
		System.out.println("Football Game Started. Enjoy the game!");
	}
}
```
```java
public class TemplatePatternDemo {
	public static void main(String[] args) {
		Game game = new Cricket();
		game.play();
		game = new Football();
		game.play();
	}
}
```

### Visitor Pattern
The purpose of a Visitor pattern is to define a new operation without introducing the modifications to an existing object structure.
A visitor design pattern is a behavioral design pattern used to decouple the logic/algorithm from the objects on which they operate. The logic is moved to separate classes called visitors. Each visitor is responsible for performing a specific operation.
Consequently, we'll make good use of the Open/Closed principle as we won't modify the code, but we'll still be able to extend the functionality by providing a new Visitor implementation.

e.g.-
```java
interface Shape {
    void accept(ShapeVisitor visitor);
}
```
```java
class Circle implements Shape {
    private final double radius;

    public Circle(final double radius) {
        this.radius = radius;
    }
    public double getRadius() {
        return radius;
    }
    @Override
    public void accept(final ShapeVisitor visitor) {
        visitor.visit(this);
    }
}
```
```java
class Square implements Shape {
    private final double length;

    public Square(final double length) {
        this.length = length;
    }
    public double getLength() {
        return length;
    }
    @Override
    public void accept(final ShapeVisitor visitor) {
        visitor.visit(this);
    }
}
```
```java
interface ShapeVisitor {
    void visit(Circle circle);
    void visit(Square square);
}
```
```java
class AreaVisitor implements ShapeVisitor {
    private double area;
    @Override
    public void visit(final Circle circle) {
        area = Math.PI * Math.pow(circle.getRadius(), 2);
    }
    @Override
    public void visit(final Square square) {
        area = 2 * square.getLength();
    }
    public double get() {
        return this.area;
    }
}
```
```java
class PerimeterVisitor implements ShapeVisitor {
    private double perimeter;
    @Override
    public void visit(final Circle circle) {
        perimeter = 2 * Math.PI * circle.getRadius();
    }
    @Override
    public void visit(final Square square) {
        perimeter = 4 * square.getLength();
    }
    public double get() {
        return this.perimeter;
    }
}
```
```java
public class TestVisitor {
	public static void main(String[] args) {
		final List<Shape> shapes = new ArrayList<>();
	
	    shapes.add(new Circle(10));
	    shapes.add(new Square(5));
	
	    final AreaVisitor areaVisitor = new AreaVisitor();
	    final PerimeterVisitor perimeterVisitor = new PerimeterVisitor();
	
	    for (Shape shape: shapes) {
	        shape.accept(areaVisitor);
	        final double area = areaVisitor.get();	
	        System.out.printf("Area of %s: %.2f%n", shape.getClass().getSimpleName(), area);
	    }	
	    for (Shape shape: shapes) {
	        shape.accept(perimeterVisitor);
	        final double perimeter = perimeterVisitor.get();	
	        System.out.printf("Perimeter of %s: %.2f%n",shape.getClass().getSimpleName(),perimeter);
	    }
	}
}
```

