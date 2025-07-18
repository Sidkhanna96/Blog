# Singleton, Factory, Strategy, Observer, Builder, Adapter, State, MVC

- Creational, Structural and Behavioural

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

# Resources:

- https://www.oodesign.com/
- https://learningdaily.dev/the-7-most-important-software-design-patterns-d60e546afb0e
