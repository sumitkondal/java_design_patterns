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
**Correct : **

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


