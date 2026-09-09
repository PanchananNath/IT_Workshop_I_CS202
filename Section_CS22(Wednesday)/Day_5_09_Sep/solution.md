# Assignment 5: Theoretical Concepts & Solutions

## Problem 1: Smart Transportation System

### Problem Explanation

This problem models a fleet management system with different types of vehicles. Each vehicle calculates its running cost differently based on distance, type, and specific attributes (like battery or gear efficiency). The goal is to design a class hierarchy where a single list of base `Vehicle` references can manage and interact with all vehicle types seamlessly.

### Theoretical Concepts

* **Inheritance:** `Car` and `Bike` inherit common properties (like `modelName`, `baseRate`, and `distanceTravelled`) from `Vehicle`. `ElectricCar` extends `Car`, creating a **multi-level inheritance hierarchy** (`Vehicle` $\rightarrow$ `Car` $\rightarrow$ `ElectricCar`).
* **Constructor Chaining (`super`):** Subclasses use `super(...)` to call the parent class constructor to initialize inherited fields before adding their own subclass-specific attributes (e.g., `batteryCapacity`).
* **Run-Time Polymorphism (Method Overriding):** Methods like `calculateRunningCost()` and `startEngine()` are declared in `Vehicle` and overridden in each subclass. When iterating over a `List<Vehicle>`, the Java Virtual Machine dynamically determines and executes the specific subclass's method at runtime based on the actual object instance.
* **Encapsulation & Boundary Checking:** Class fields are kept private and exposed via getters and setters. Input boundaries (such as preventing negative distances) are validated in constructors and setters.

### Pseudocode

```text
CLASS Vehicle
    PRIVATE modelName, baseRate, distanceTravelled

    CONSTRUCTOR Vehicle(modelName, baseRate, distanceTravelled)
        SET this.modelName = modelName
        SET this.baseRate = baseRate
        SET this.distanceTravelled = MAX(0, distanceTravelled)
    END CONSTRUCTOR

    FUNCTION calculateRunningCost()
        RETURN baseRate * distanceTravelled
    END FUNCTION

    FUNCTION displayDetails()
        PRINT modelName, distanceTravelled, baseRate, calculateRunningCost()
    END FUNCTION
END CLASS

CLASS Car EXTENDS Vehicle
    PRIVATE seatingCapacity

    CONSTRUCTOR Car(modelName, baseRate, distanceTravelled, seatingCapacity)
        CALL super(modelName, baseRate, distanceTravelled)
        SET this.seatingCapacity = seatingCapacity
    END CONSTRUCTOR

    OVERRIDE FUNCTION calculateRunningCost()
        RETURN super.calculateRunningCost() + FLAT_MAINTENANCE_SURCHARGE
    END FUNCTION
END CLASS

CLASS ElectricCar EXTENDS Car
    PRIVATE batteryCapacity, costPerKWh

    CONSTRUCTOR ElectricCar(modelName, baseRate, distanceTravelled, seatingCapacity, batteryCapacity, costPerKWh)
        CALL super(modelName, baseRate, distanceTravelled, seatingCapacity)
        SET this.batteryCapacity = batteryCapacity
        SET this.costPerKWh = costPerKWh
    END CONSTRUCTOR

    OVERRIDE FUNCTION calculateRunningCost()
        energyConsumed = distanceTravelled * KWH_PER_KM_RATE
        RETURN energyConsumed * costPerKWh
    END FUNCTION
END CLASS

CLASS Bike EXTENDS Vehicle
    PRIVATE hasGear

    OVERRIDE FUNCTION calculateRunningCost()
        RETURN baseRate * DISCOUNT_FACTOR * distanceTravelled
    END FUNCTION
END CLASS

// Demonstration Procedure
PROCEDURE Main()
    CREATE list of Vehicle references
    ADD Car, ElectricCar, and Bike instances to the list
    
    FOR EACH vehicle IN list
        CALL vehicle.startEngine()       // Executed polymorphically
        CALL vehicle.displayDetails()    // Executed polymorphically
    END FOR
END PROCEDURE

```

---

## Problem 2: Employee Payroll System

### Problem Explanation

This problem requires designing an automated payroll calculation system for different employee roles (full-time, part-time, contract) and providing flexible bonus calculation strategies based on company policies.

### Theoretical Concepts

* **Compile-Time Polymorphism (Method Overloading):** Multiple methods in the same class share the same name (`calculateBonus`) but differ in parameter lists (number or type of parameters). The compiler decides which version to invoke based on arguments passed:
1. `calculateBonus(fixedAmount)`
2. `calculateBonus(percentage)`
3. `calculateBonus(percentage, performanceRating)`


* **Run-Time Polymorphism (Method Overriding):** `calculateSalary()` is defined in `Employee` and overridden in `FullTimeEmployee` (adds allowances), `PartTimeEmployee` (hours $\times$ rate), and `ContractEmployee` (fixed contract fee).
* **Abstract Data Access & Generalization:** Treating all specific employee types as generic `Employee` instances allows processing company-wide payroll through unified iteration.

### Pseudocode

```text
CLASS Employee
    PRIVATE name, id, baseSalary

    CONSTRUCTOR Employee(name, id, baseSalary)
        SET this.name = name, this.id = id, this.baseSalary = baseSalary
    END CONSTRUCTOR

    FUNCTION calculateSalary()
        RETURN baseSalary
    END FUNCTION

    // Overloaded Bonus Method 1: Fixed Bonus
    FUNCTION calculateBonus(fixedAmount)
        RETURN fixedAmount
    END FUNCTION

    // Overloaded Bonus Method 2: Percentage Bonus
    FUNCTION calculateBonus(percentage)
        RETURN (calculateSalary() * percentage) / 100
    END FUNCTION

    // Overloaded Bonus Method 3: Percentage + Rating Bonus
    FUNCTION calculateBonus(percentage, performanceRating)
        RETURN calculateBonus(percentage) * performanceRating
    END FUNCTION
END CLASS

CLASS FullTimeEmployee EXTENDS Employee
    PRIVATE allowance

    OVERRIDE FUNCTION calculateSalary()
        RETURN baseSalary + allowance
    END FUNCTION
END CLASS

CLASS PartTimeEmployee EXTENDS Employee
    PRIVATE hoursWorked, hourlyRate

    OVERRIDE FUNCTION calculateSalary()
        RETURN hoursWorked * hourlyRate
    END FUNCTION
END CLASS

CLASS ContractEmployee EXTENDS Employee
    PRIVATE contractAmount

    OVERRIDE FUNCTION calculateSalary()
        RETURN contractAmount
    END FUNCTION
END CLASS

// Demonstration Procedure
PROCEDURE Main()
    CREATE list of Employee references
    ADD FullTimeEmployee, PartTimeEmployee, and ContractEmployee to list

    FOR EACH emp IN list
        CALL emp.displayDetails() // Invokes dynamic calculateSalary()
    END FOR

    // Demonstrate Compile-Time Overloading
    emp1 = FullTimeEmployee(...)
    bonus1 = emp1.calculateBonus(500.0)             // Fixed amount
    bonus2 = emp1.calculateBonus(10)                // Percentage-based
    bonus3 = emp1.calculateBonus(10, 1.25)          // Performance-adjusted
END PROCEDURE

```
