# OOP Design Patterns:

- Creational: Singleton √, Factory √, Builder √
- Behavioral: Strategy √, Observer √
- Structural: Adapter √, Decorator √
- --- Command, state, facade, iterator, MVC

# Creational:

## Singleton:

- Only one instance of a class is created
  - Constructor is private
  - There is a static variable and method for accessing the instance
- Use Case:
  - Logger
  - Configuration for an application
  - Whenever a global access is required throughout the application

```java
class Singleton {
    private static Singleton instance;

    private Singleton() { }

    private static Singleton getInstance(){
        if (instance == null) {
            instance = new Singleton();
        }

        return instance;
    }

    public void doSomething() { }
}

Singleton().getInstance().doSomething();
```

## Factory Method:

- Objects are created using methods instead of constructor
  - Creates objects without exposing instantiation logic to the client
  - Refers to newly created object through a **common** interface
- Use Case:
  - Creating shapes on lucidchart/excalidraw -> Instead of individually creating classes -> Abstract this logic into another class

```java
public class ShapeFactory{
  public Shape createShape(String ID){
    if (ID == "Circle"){
      return new Circle()
    }
    else if (ID == "Triangle") {
      return new Triangle()
    }
    else {
      return null
    }
  }
}

// Improvement on above -> Called Reflection -> Giving ability to register -> Making updating logic dynamic using hashmap

public class Shape {
  private Hashmap registeredShape = new Hashmap();

  public void registerShape(String ID, Class shapeClass){
      registeredShape.put(ID, shapeClass)
  }

  public Shape createShape(String ID){
    try {
      Class shapeClass = (Class) registeredShape.get(ID)
      return (Shape) shapeClass.getDeclaredConstructor().newInstance();
    } catch(Exception e){
      throw new RuntimeException("Failed to create shape: " + shapeId, e);
    }
  }
}
```

## Builder

--> Not sure why we need this in programming - like how does normal object creation become more difficult - like what problem is it really solving ?

- Object created can be complex (Made up of several sub objects) and requires elaborate construction process -> Use Builder pattern to simplify this
- Factory returns object in one go -> Builder creates an object step by step
- Use Case:
  - VehicleBuilder -> Can build car, bike, scooter etc. Can provide interface to use the same parts (objects) but different set of rules.

Structure:

- Builder - Abstract interface - would be needed to create parts of the product
- ConcreteBuilder - Implments the Builder Interface. Interface for saving the product and actually builds the product
- Director - Complex object created using the Builder interface
- Product - Complex object representation

```java

// Client
public class Client {
  ASCIIConverter asciiBuilder = new ASCIIConverter(); // Concrete Builder
  RTFReader rtfReader = new RTFReader(asciiBuilder); // Director being passed Concrete Builder -> It houses the Builder
  ASCIIText asciiText = asciiBuilder.getResult(); // Product
}

// Director
class RTFReader {
  TextConverter builder;
  RTFReader(TextConverter obj) {
    builder.obj
  }
  void parseRTF(Document doc) {
    while((t=doc.getNextToken()) != EOF){
      builder.convertCharacter(t) // Concrete Builder function
    }
  }
}

// Concrete Builder
class ASCIIConverter extends TextConverter {
  ASCIIText asciiTextObj; // Product

  object void convertCharacter(char c){
    char asciiChar = new Character(c).charValue();
    asciiTextObj.append(asciiChar)
  }

  ASCIIText getResult() { return asciiTextObj }
}

// Product
class ASCIIText {
  public void append(char c) { //Implm }
}

// Builder
class abstract class TextConverter {
  abstract void convertCharacter(char c);
}
```

Summary:

- Director parses through the document leveraging the functionality within Concrete Builder which leverages the interface of the abstract class and also uses the Product class to define the output

-

# Behavioural: Algorithms and the assignment of responsibilities

## Strategy

- Algorithmic behaviours are separated out into different classes and are implemented through a common interface
- Each strategy class is then invoked by a context (Your main object class) based on their requirements
- Use Case:
  - This helps maintain a sense of consistency
  - A Robot has different behaviors and then based on whatever its sensors interact with can invoke a given strategy

```java

// Strategy Interface
class interface IBehavior{
  public int moveCommand();
}

// Strategy Implementation
public class AggressiveBehavior implements IBehaviors {
  public int moveCommand(){
    return 100;
  }
}

public class DefensiveBehavior implements IBehaviors {
  public int moveCommand() {
    return -50;
  }
}

// Context Class
public class Robot {
  IBehavior behavior;

  public Robot(){}

  public void setBehavior(IBehavior behavior) {
    this.behavior = behavior;
  }

  public void move(){
    this.behaviour.moveCommand();
  }
}

public class Main {
  public static void main(String[] args) {
    Robot r1 = new Robot();
    Robot r2 = new Robot();

    r1.setBehavior(new AggresiveBehavior);
    r2.setBehavior(new DefensiveBehavior);

    r1.move() // Notice how regardless of the behaviour for strategy passed it will trigger the proper method of the robot class

    // Some logic and then set new behavior
  }
}

```

## Observer (Subscription Mechanism)

- One to Many relation between objects, one object changes state
- Subscription mechanism
- Use Case:
  - Youtube Channel
  - Group Chats

```java
import jave.util.*;

interface Observer{
  void update(String channelName, string videoTitle);
}

interface Subject {
  void subscribe(Observer observer);
  void notifyObserverse();
}

public class YoutubeViewer implements Observer{
  public String viewerName;

  public YoutubeViewer(String viewerName){
    this.viewerName = viewerName;
  }

  public void update(String channelName, String videoTitle){
    System.out.println(channelName + " " + videoTitle + "Uploaded")
  }
}

public class YoutubeChannel implements Subject{
  private String channelName;
  private String latestVideo;
  private List<Observer> subscribers;

  public YoutubeChannel(String channelName){
    this.channelName = channelName;
    this.subscribers = new ArrayList<>();
  }

  public void subscribe(Observer observer){
    this.subscribers.add(observer)
  }

  public void notifyObserver(){
    for(Observer subscriber: this.subscribers){
      subsriber.update(this.channelName, this.latestVideo)
    }
  }

  public void uploadVideo(String video){
    this.latestVideo = video
    this.notifyObserver()
  }
}

public class ObserverPatternDemo{
  public static void main(String[] args){
    YouTubeChannel techChannel = new YouTubeChannel("TechTips");
    YouTubeViewer alice = new YouTubeViewer("Alice");

    techChannel.subscribe(alice);

    techChannel.uploadVideo("10 Best Programming Languages in 2024");
  }
}

```

- Summary -> The Subscribers are attched to the publishers state management array, Publisher is calling the subscriber classes notification system

# Structural: Assemble objects and classes in larger structures

## Adapter

- Makes incompatible interfaces work together
- Use Case:
  - Change Video Format

```java

interface EuropeanPlug {
  void plugIn();
}

class AmericanPlug{
  public void insertPlug(){
    System.out.println("American plug inserted");
  }
}

class PlugAdapter implements EuropeanPlug {
  private AmericanPlug americanPlug;

  public plugAdapter(AmericanPlug americanPlug){
    this.americanPlug = americanPlug;
  }

  public void plugIn(){
    americanPlug.insertPlug();
  }
}

public class AdapterDemo {
  public static void main(String[] args){
    AmericanPlug americanPlug = new AmericanPlug();
    EuropeanPlug adapter = new PlugAdapter(americanPlug);

    adapter.plugIn();
  }
}
```

- Summary: It enables one class (American Plug) interface to behave like another class (European Plug) by using an adapter class (PlugAdapter) in the middle

## Decorator:

- Extend an objects functionality at runtime - normally done at compile time using inheritance
- Allows adding behaviour to individual objects dynamically without affecting the behaviour of other objects belonging to the same class

- Structure:
- Component Interface
- Concrete Component
- Decorator Interface/Abstract
- ConcreteDecorator Interface/Abstract

```java
// Component Interface
public interface Coffee{
  double getCost();
}

// Decorator Interface/Abstract
public abstract CoffeeDecorator implements Coffee {
  protected Coffee decoratedCofee;

  public CoffeeDecorator(Coffee decoratedCoffee) {
    this.decoratedCofee = decoratedCoffee;
  }

  public double getCost(){
    return decoratedCoffee.getCost();
  }
}

public class PlainCoffee implements Coffee {
    public double getCost() {
        return 2.0;
    }
}

public class MilkDecorator extends CoffeeDecorator {
    public MilkDecorator(Coffee decoratedCoffee) {
        super(decoratedCoffee);
    }

    public double getCost() {
        return decoratedCoffee.getCost() + 0.5;
    }
}

public class Main{
  public static void Main(String[] args){
    Coffee coffee = new PlainCofee();
    System.out.println(coffee.getCost()); // 2

    Coffee milkCoffee = new MilkDecorator(new PlainCoffee());
    System.out.println(milkCoffee.getCost()); // 2.5
  }
}

```

- Summary: The object created from the decorator can pick up data from the original interface object and augment to it

# Resources:

- https://www.oodesign.com/
- https://learningdaily.dev/the-7-most-important-software-design-patterns-d60e546afb0e
- Chat GPT
