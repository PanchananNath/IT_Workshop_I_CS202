

## Q1 — Pseudocode

### Class `Person`
1. Define a class `Person`.
2. Declare an integer field `age`.
3. Create a constructor that receives `age` and initializes the field.
4. Define `eat()`.
5. Define `sleep()`.

### Interface `Driver`
1. Declare abstract function `driveCar()`.
2. Declare abstract function `driveBike()`.

### Interface `Singer`
1. Declare abstract function `riyaz()`.
2. Declare abstract function `sing()`.

### Class `Employee`
1. Make `Employee` inherit from `Person`.
2. Make `Employee` implement `Driver` and `Singer`.
3. Define `driveCar()`:
   - If `age < 40`, return `10`.
   - Otherwise return `0`.
4. Define `driveBike()`:
   - If `age < 40`, return `5`.
   - Otherwise return `0`.
5. Define `riyaz()`.
6. Define `sing()`:
   - If `age < 20`, return `15`.
   - Otherwise return `0`.
7. Define `officeWork()`:
   - If `age < 40`, return `20`.
   - Otherwise return `10`.

### Runtime polymorphism
1. Create an `Employee` object.
2. Store it in a `Driver` reference.
3. Through the `Driver` reference, call only `driveCar()` and `driveBike()`.
4. Store the same/different `Employee` object in a `Singer` reference.
5. Through the `Singer` reference, call only `riyaz()` and `sing()`.
6. This demonstrates runtime polymorphism while restricting access according to the reference type.

The assignment specifically requires an Employee acting as a Driver not to access Singer-related functions, and an Employee acting as a Singer not to access Driver-related functions. 

---

## Q2 — Pseudocode

1. Create an array capable of storing five `Employee` objects.
2. Create an integer array for the corresponding EIF values.
3. Create five Employee objects with different ages.
4. For every Employee `e`, calculate:

   `EIF = e.driveCar() + e.driveBike() + e.sing() + e.officeWork()`

5. Store each EIF in the corresponding position of the EIF array.
6. Define `sort(Employee[] employees, int[] eif)`.
7. Sort the Employee objects in increasing order of EIF.
8. Whenever two Employee objects are swapped, swap their corresponding EIF values as well.
9. Display the sorted Employee objects and their EIF values.

The assignment defines EIF as the sum of `driveCar()`, `driveBike()`, `sing()`, and `officeWork()`, and asks for increasing-order sorting using a `sort()` function that receives both arrays. 

---

# Full Java Code

```java
interface Driver {
    int driveCar();
    int driveBike();
}

interface Singer {
    void riyaz();
    int sing();
}

class Person {
    protected int age;

    Person(int age) {
        this.age = age;
    }

    void eat() {
        System.out.println("Person is eating.");
    }

    void sleep() {
        System.out.println("Person is sleeping.");
    }
}

class Employee extends Person implements Driver, Singer {

    Employee(int age) {
        super(age);
    }

    @Override
    public int driveCar() {
        if (age < 40) {
            return 10;
        }
        return 0;
    }

    @Override
    public int driveBike() {
        if (age < 40) {
            return 5;
        }
        return 0;
    }

    @Override
    public void riyaz() {
        System.out.println("Employee is doing riyaz.");
    }

    @Override
    public int sing() {
        if (age < 20) {
            return 15;
        }
        return 0;
    }

    int officeWork() {
        if (age < 40) {
            return 20;
        }
        return 10;
    }

    int getAge() {
        return age;
    }
}

public class Assignment7 {

    // Demonstrates runtime polymorphism.
    static void demonstratePolymorphism(Employee employee) {

        // Employee acting as Driver.
        Driver driver = employee;

        System.out.println("Employee acting as Driver:");
        System.out.println("Car driving points: " + driver.driveCar());
        System.out.println("Bike driving points: " + driver.driveBike());

        // The following would NOT compile because driver is a Driver reference:
        // driver.sing();
        // driver.riyaz();

        // Employee acting as Singer.
        Singer singer = employee;

        System.out.println("\nEmployee acting as Singer:");
        singer.riyaz();
        System.out.println("Singing points: " + singer.sing());

        // The following would NOT compile because singer is a Singer reference:
        // singer.driveCar();
        // singer.driveBike();
    }

    // Calculates EIF for one Employee.
    static int calculateEIF(Employee employee) {
        return employee.driveCar()
                + employee.driveBike()
                + employee.sing()
                + employee.officeWork();
    }

    // Sorts employees and their corresponding EIF values
    // in increasing order of EIF.
    static void sort(Employee[] employees, int[] eif) {

        for (int i = 0; i < employees.length - 1; i++) {

            for (int j = 0; j < employees.length - 1 - i; j++) {

                if (eif[j] > eif[j + 1]) {

                    // Swap EIF values.
                    int tempEIF = eif[j];
                    eif[j] = eif[j + 1];
                    eif[j + 1] = tempEIF;

                    // Swap corresponding Employee objects.
                    Employee tempEmployee = employees[j];
                    employees[j] = employees[j + 1];
                    employees[j + 1] = tempEmployee;
                }
            }
        }
    }

    public static void main(String[] args) {

        // Q1: Runtime polymorphism demonstration.
        Employee employee = new Employee(25);
        demonstratePolymorphism(employee);

        // Q2: Create five Employee objects with different ages.
        Employee[] employees = {
            new Employee(15),
            new Employee(25),
            new Employee(45),
            new Employee(35),
            new Employee(18)
        };

        // Calculate EIF values.
        int[] eif = new int[employees.length];

        for (int i = 0; i < employees.length; i++) {
            eif[i] = calculateEIF(employees[i]);
        }

        // Display before sorting.
        System.out.println("\nBefore sorting:");
        for (int i = 0; i < employees.length; i++) {
            System.out.println(
                "Age = " + employees[i].getAge()
                + ", EIF = " + eif[i]
            );
        }

        // Sort in increasing order of EIF.
        sort(employees, eif);

        // Display after sorting.
        System.out.println("\nAfter sorting by increasing EIF:");
        for (int i = 0; i < employees.length; i++) {
            System.out.println(
                "Age = " + employees[i].getAge()
                + ", EIF = " + eif[i]
            );
        }
    }
}
```

## Notes

- The question says the interface functions "return" values for `driveCar()`, `driveBike()`, and `sing()`, so the Java interfaces use `int` return types for those methods.
- `riyaz()` is implemented as a `void` method because the question does not specify a return value for it.
- The sample uses five different ages: `15, 25, 45, 35, 18`.
- The `Driver` and `Singer` references demonstrate interface-based runtime polymorphism: each reference exposes only the methods declared by its interface.
