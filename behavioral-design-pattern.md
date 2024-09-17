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

