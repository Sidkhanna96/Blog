# Creational: Singleton √, Factory √, Builder √

# Behavioral: Strategy, Observer

# Structural: Adapter, Decorator

# --- Command, state, facade, MVC

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
}

public class Main {
  public static void main(String[] args) {
    Robot r1 = new Robot();
    Robot r2 = new Robot();

    r1.setBehavior(new AggresiveBehavior);
    r2.setBehavior(new DefensiveBehavior);

    // Some logic and then set new behavior
  }
}


```

## Observer

# Structural

## Adapter

## Decorator

# Resources:

- https://www.oodesign.com/
- https://learningdaily.dev/the-7-most-important-software-design-patterns-d60e546afb0e
