# Java Refresher

## Variables:

```java
char c = 'a';
int a = 0;
float f = 4.5f;
double d = 4.5;
String s = "popo";
```

## Objects and Functions

- Note: All functions in java need to be inside classes (Functions == methods)
  - Functions have attributes / arguments - they have data types (primitives or Objects) assigned to them.
- Some level on encapsulation (Private, public, protected on classes, methods and fields)
- return type on methods

```java
public class Robot{
    String name;
    int[] position;

    // Constructor - no return type
    public Robot(String name){
        this.name = name;
        this.position = new int[]{0,0};
    }

    public void move(int[] dir){
        this.position[0] = this.position[0] + dir[0];
        this.position[1] = this.position[1] + dir[1];
    }
}
```

## Static:

- all methods in an object should be non-static if they're changing the fields of the object
- if methods are static they're not mutating the fields (kind of like utility function doing some math) - belong to the object not class

## Pass by Value vs Pass by Referece:

- all primitive parameters to functions are passed by value
- all objects are passed by reference
  - if you change the argument value even if you assign it to another field in the object it will change the original object - this is because copy of reference is provided to the argument - and any change to the reference changes the original object

# Resource:

- https://www.learnjavaonline.org/
