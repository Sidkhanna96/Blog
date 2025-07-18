# OOP:

- Classes:
  - Help create instance of object (Can create Animal class and then instance of dog, cat etc. can be reused)
  - Structure of classes:
    - contructor
    - methods
    - attributes (Variables)
    - encapsulation type (private or public)
    - data type
    - static -> Can access a classes variable or method directly without needing a class
    - this -> Refer to instance values
    - super() -> Inherit everything from the extending abstract class - it is good hygiene

```java
class Animal {
    String name;
    int age;

    // Constructor
    public Animal(String name, int age) {
        this.name = name;
        this.age = age;
    }

    // Return type void
    public void noise(String sound){
        System.out.println("The animal " + this.name + " makes the sound " + sound);
    }

    public String getName(){
        return this.name;
    }
}
```

# Abstraction, Encapsulation, inheritance and polymorphism

# Abstraction:

- Hides the implementation mechanism and classes only reveal necessary implementation details
- Can be leveraged using Abstract classes or interfaces

- Abstract Classes:

```java
abstract class Animal {
    String name;
    int age;

    // Constructor
    public Animal(String name, int age) {
        this.name = name;
        this.age = age;
    }

    // Return type void
    public void noise(String sound){
        System.out.println("The animal " + this.name + " makes the sound " + sound);
    }

    public String getName(){
        return this.name;
    }
}

class Cat extends Animal {
    String attack;

    public Cat(String name, int age){
        super(name, age);
    }

    // Overriding the method
    public void noise(String sound){
        System.out.println(name + " meows: Meow!");
    }

    // New method part of cat but not Animal
    public String attack(String attack){
        this.attack = attack;
        return this.attack;
    }
}
```

- Interface Classes: These are just contract that the fields implementing the interface need to have associated values

```java
interface Animal {
    void noise(String sound)
    void sleep()
}

class Cat implements Animal{
    String name;

    public void Cat(String name){
        this.name = name
    }

    public void noise(String sound){
        System.out.println("This is a sound: " + sound + this.name);
    }
}
```

# Encapsulation:

- Hiding data, making it selectively accessible
- public or private

```java
class Animal{
    private String name;

    private String helperName(){
        return " is the name"
    }

    public Animal(String name){
        this.name = name
    }

    // name attribute only accessible through the getName function
    public String getName(){
        return this.name + this.helperName()
    }
}
```

# Inheritance

- One class can inherit properties of the parent method - using extend on abstract classes
- Inherits attributes and methods - does not inherit private attributes/methods

```java
// Same as Abstract class

```

# Polymorphism

- Many forms of the same function depending on the instantiation version of the class

```java
abstract class Animal {
    protected String name;

    public Animal(String name) {
        this.name = name;
    }

    // Abstract method - each subclass implements differently
    public abstract void makeSound();
}

class Cat extends Animal {
    private int age;

    public Cat(String name, int age){
        super(name);
        this.age = age;
    }

    // poly
    public void makeSound(){
        System.out.println(name + " says: meow");
    }
}


class Dog extends Animal {
    public Dog(String name) {
        super(name);
    }

    public void makeSound() {
        System.out.println(name + " says: woof");
    }
}

public class Main {
    public static void main(String[] args) {
        // Same reference type (Animal), different object types
        Animal[] animals = {
            new Cat("Whiskers", 3),
            new Dog("Buddy"),
        };

        // Same method call, different behavior (POLYMORPHISM)
        for (Animal animal : animals) {
            animal.makeSound();  // Different output for each animal type
        }
    }
}

```
