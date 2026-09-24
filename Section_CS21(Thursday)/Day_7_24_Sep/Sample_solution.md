
## Q1. Method Overloading – Addition

### Basic idea

**Method overloading** means having multiple methods with the same name but different parameter lists.

Here, we create three `add()` methods:

1. `add()` – takes no parameters. It asks the user for two numbers and returns their sum as an `int`.
2. `add(int a, int b)` – takes two integers and returns their sum.
3. `add(int a, int b, int c)` – takes three integers and returns their sum.

Java chooses the appropriate overloaded method from the arguments supplied.

### Pseudocode

```text
START
Create a class named Addition

Create add()
    Ask user for two integers
    Read the two integers
    Return their sum

Create add(a, b)
    Return a + b

Create add(a, b, c)
    Return a + b + c

In main:
    Create an Addition object
    Call add() and display result
    Call add(10, 20) and display result
    Call add(10, 20, 30) and display result
END
```

### Java code

```java
import java.util.Scanner;

public class Addition {

    private Scanner scanner = new Scanner(System.in);

    int add() {
        System.out.print("Enter first number: ");
        int a = scanner.nextInt();

        System.out.print("Enter second number: ");
        int b = scanner.nextInt();

        return a + b;
    }

    int add(int a, int b) {
        return a + b;
    }

    int add(int a, int b, int c) {
        return a + b + c;
    }

    public static void main(String[] args) {
        Addition obj = new Addition();

        System.out.println("Result of add(): " + obj.add());
        System.out.println("Result of add(10, 20): " + obj.add(10, 20));
        System.out.println("Result of add(10, 20, 30): "
                + obj.add(10, 20, 30));
    }
}
```

### Example output

```text
Enter first number: 5
Enter second number: 7
Result of add(): 12
Result of add(10, 20): 30
Result of add(10, 20, 30): 60
```

## Q2. Method Overriding – Shape and Rectangle

### Basic idea

**Method overriding** happens when a subclass provides its own implementation of a method already defined in its parent class.

The assignment asks for a `Shape` class with a `getArea()` method and a `Rectangle` subclass that overrides `getArea()` to calculate the rectangle's area.

For a rectangle:

```text
Area = length × width
```

### Pseudocode

```text
START
Create class Shape
    Create getArea()
        Return 0

Create subclass Rectangle that extends Shape
    Store length and width
    Create constructor to initialize length and width

    Override getArea()
        Return length * width

In main:
    Create Rectangle object
    Call getArea()
    Display the area
END
```

### Java code

```java
class Shape {

    double getArea() {
        return 0;
    }
}

class Rectangle extends Shape {

    private double length;
    private double width;

    Rectangle(double length, double width) {
        this.length = length;
        this.width = width;
    }

    @Override
    double getArea() {
        return length * width;
    }
}

public class ShapeDemo {

    public static void main(String[] args) {

        Rectangle rectangle = new Rectangle(10, 5);

        System.out.println("Length: 10");
        System.out.println("Width: 5");
        System.out.println("Area of Rectangle: " + rectangle.getArea());
    }
}
```

### Example output

```text
Length: 10
Width: 5
Area of Rectangle: 50.0
```

## Quick difference: Overloading vs Overriding

| Feature | Method Overloading | Method Overriding |
|---|---|---|
| Main idea | Same method name, different parameters | Child class provides a new implementation |
| Classes | Usually within the same class | Requires inheritance |
| Example here | `add()`, `add(int,int)`, `add(int,int,int)` | `Rectangle.getArea()` overrides `Shape.getArea()` |
| Selection | Based on method arguments | Based on the object/class relationship |



