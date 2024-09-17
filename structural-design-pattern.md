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

Without Bridge Pattern:					            

<img width="215" alt="image" src="https://github.com/user-attachments/assets/febfbb9a-5b00-4aac-a435-f87cc789ab85">       With Bridge – between Vehicle and Workshop:	<img width="287" alt="image" src="https://github.com/user-attachments/assets/61d33596-8dc0-480b-b54d-18a052d0d753">

                

