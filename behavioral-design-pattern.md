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

