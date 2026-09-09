# Solutions for IT Workshop I (CS202) - Assignment 5

## Problem 1: Smart Transportation System

### Java Implementation

```java
import java.util.ArrayList;

// Base class Vehicle
class Vehicle {
    private String modelName;
    private double baseRate;
    private double distanceTravelled;

    public Vehicle(String modelName, double baseRate, double distanceTravelled) {
        this.modelName = modelName;
        this.baseRate = baseRate;
        this.distanceTravelled = Math.max(0, distanceTravelled); // Ensure non-negative distance
    }

    // Getters and Setters
    public String getModelName() { return modelName; }
    public double getBaseRate() { return baseRate; }
    public double getDistanceTravelled() { return distanceTravelled; }

    public void setDistanceTravelled(double distanceTravelled) {
        this.distanceTravelled = Math.max(0, distanceTravelled);
    }

    // Method to calculate running cost (to be overridden)
    public double calculateRunningCost() {
        return baseRate * distanceTravelled;
    }

    // Overridden display details method
    public void displayDetails() {
        System.out.println("-------------------------------------------");
        System.out.println("Vehicle Type       : " + this.getClass().getSimpleName());
        System.out.println("Model Name         : " + modelName);
        System.out.println("Distance Travelled : " + distanceTravelled + " km");
        System.out.println("Base Rate          : $" + baseRate + "/km");
        System.out.printf("Total Running Cost : $%.2f%n", calculateRunningCost());
    }

    public void startEngine() {
        System.out.println(modelName + ": Engine started.");
    }
}

// Subclass: Car
class Car extends Vehicle {
    private int seatingCapacity;

    public Car(String modelName, double baseRate, double distanceTravelled, int seatingCapacity) {
        super(modelName, baseRate, distanceTravelled);
        this.seatingCapacity = seatingCapacity;
    }

    @Override
    public double calculateRunningCost() {
        // Cars have a flat maintenance surcharge per trip plus distance cost
        double maintenanceFee = 15.00;
        return super.calculateRunningCost() + maintenanceFee;
    }

    @Override
    public void displayDetails() {
        super.displayDetails();
        System.out.println("Seating Capacity   : " + seatingCapacity + " seats");
    }

    @Override
    public void startEngine() {
        System.out.println(getModelName() + " (Car): Vroom! Fuel engine started.");
    }
}

// Subclass: ElectricCar inheriting from Car (Demonstrating Multi-level Inheritance)
class ElectricCar extends Car {
    private double batteryCapacity; // in kWh
    private double costPerKWh;

    public ElectricCar(String modelName, double baseRate, double distanceTravelled, int seatingCapacity, double batteryCapacity, double costPerKWh) {
        // Constructor chaining using super()
        super(modelName, baseRate, distanceTravelled, seatingCapacity);
        this.batteryCapacity = batteryCapacity;
        this.costPerKWh = costPerKWh;
    }

    @Override
    public double calculateRunningCost() {
        // Electric car running cost based on electricity consumption (approx 0.15 kWh/km)
        double energyConsumed = getDistanceTravelled() * 0.15;
        return energyConsumed * costPerKWh;
    }

    @Override
    public void displayDetails() {
        super.displayDetails();
        System.out.println("Battery Capacity   : " + batteryCapacity + " kWh");
        System.out.println("Charging Rate      : $" + costPerKWh + "/kWh");
    }

    @Override
    public void startEngine() {
        System.out.println(getModelName() + " (Electric Car): Silent boot! Electric system ready.");
    }
}

// Subclass: Bike
class Bike extends Vehicle {
    private boolean hasGear;

    public Bike(String modelName, double baseRate, double distanceTravelled, boolean hasGear) {
        super(modelName, baseRate, distanceTravelled);
        this.hasGear = hasGear;
    }

    @Override
    public double calculateRunningCost() {
        // Bikes have higher efficiency (20% discount on base rate)
        return getBaseRate() * 0.8 * getDistanceTravelled();
    }

    @Override
    public void displayDetails() {
        super.displayDetails();
        System.out.println("Geared Bike        : " + (hasGear ? "Yes" : "No"));
    }

    @Override
    public void startEngine() {
        System.out.println(getModelName() + " (Bike): Kick/Button start! Bike engine revved.");
    }
}

// Main Test Class
public class Main {
    public static void main(String[] args) {
        System.out.println("==================================================");
        System.out.println("       SMART TRANSPORTATION SYSTEM DEMO           ");
        System.out.println("==================================================");

        // Test Case a: Normal Car
        Car sedan = new Car("Honda City", 0.50, 120.0, 5);

        // Test Case b: ElectricCar
        ElectricCar tesla = new ElectricCar("Tesla Model 3", 0.00, 150.0, 5, 75.0, 0.18);

        // Test Case c: Bike
        Bike motorcycle = new Bike("Yamaha R15", 0.30, 80.0, true);

        // Test Case d: Store using Vehicle polymorphic references
        ArrayList<Vehicle> fleet = new ArrayList<>();
        fleet.add(sedan);
        fleet.add(tesla);
        fleet.add(motorcycle);

        System.out.println("\n--- Processing Fleet (Run-Time Polymorphism) ---");
        for (Vehicle v : fleet) {
            v.startEngine();        // Polymorphic method call
            v.displayDetails();     // Dynamic method dispatch
        }

        // Test Case e: Verify rate changes with different speeds/distances
        System.out.println("\n==================================================");
        System.out.println("   Test Case e: Updating Distance & Re-evaluating   ");
        System.out.println("==================================================");
        tesla.setDistanceTravelled(300.0);
        tesla.displayDetails();

        // Test Case f: Boundary condition test (Zero distance travelled)
        System.out.println("\n==================================================");
        System.out.println("   Test Case f: Boundary Case (0 km distance)     ");
        System.out.println("==================================================");
        Car stationaryCar = new Car("Toyota Corolla", 0.45, 0.0, 5);
        stationaryCar.displayDetails();
    }
}



# Solutions for IT Workshop I (CS202) - Assignment 5

## Problem 2: Employee Payroll System

### Java Implementation

```java
import java.util.ArrayList;

// Base class Employee
class Employee {
    private String name;
    private int id;
    private double baseSalary;

    public Employee(String name, int id, double baseSalary) {
        this.name = name;
        this.id = id;
        this.baseSalary = baseSalary;
    }

    // Getters and Setters
    public String getName() { return name; }
    public int getId() { return id; }
    public double getBaseSalary() { return baseSalary; }

    public void setBaseSalary(double baseSalary) {
        this.baseSalary = baseSalary;
    }

    // Method to calculate salary (to be overridden)
    public double calculateSalary() {
        return baseSalary;
    }

    // Overloaded calculateBonus methods
    // 1. Fixed bonus
    public double calculateBonus(double fixedAmount) {
        return fixedAmount;
    }

    // 2. Percentage-based bonus
    public double calculateBonus(int percentage) {
        return (calculateSalary() * percentage) / 100.0;
    }

    // 3. Percentage + Performance rating multiplier
    public double calculateBonus(int percentage, double performanceRating) {
        double baseBonus = calculateBonus(percentage);
        return baseBonus * performanceRating; // Multiplier based on rating (e.g., 1.2 for high performance)
    }

    public void displayDetails() {
        System.out.println("-------------------------------------------");
        System.out.println("Employee Type : " + this.getClass().getSimpleName());
        System.out.println("ID            : " + id);
        System.out.println("Name          : " + name);
        System.out.printf("Total Salary  : $%.2f%n", calculateSalary());
    }
}

// Subclass: FullTimeEmployee
class FullTimeEmployee extends Employee {
    private double allowance;

    public FullTimeEmployee(String name, int id, double baseSalary, double allowance) {
        super(name, id, baseSalary);
        this.allowance = allowance;
    }

    @Override
    public double calculateSalary() {
        return getBaseSalary() + allowance;
    }

    @Override
    public void displayDetails() {
        super.displayDetails();
        System.out.println("Allowance     : $" + allowance);
    }
}

// Subclass: PartTimeEmployee
class PartTimeEmployee extends Employee {
    private int hoursWorked;
    private double hourlyRate;

    public PartTimeEmployee(String name, int id, int hoursWorked, double hourlyRate) {
        super(name, id, 0); // Base salary is 0, calculated per hour
        this.hoursWorked = hoursWorked;
        this.hourlyRate = hourlyRate;
    }

    @Override
    public double calculateSalary() {
        return hoursWorked * hourlyRate;
    }

    @Override
    public void displayDetails() {
        super.displayDetails();
        System.out.println("Hours Worked  : " + hoursWorked + " hrs");
        System.out.println("Hourly Rate   : $" + hourlyRate + "/hr");
    }
}

// Subclass: ContractEmployee
class ContractEmployee extends Employee {
    private double contractAmount;

    public ContractEmployee(String name, int id, double contractAmount) {
        super(name, id, 0);
        this.contractAmount = contractAmount;
    }

    @Override
    public double calculateSalary() {
        return contractAmount;
    }

    @Override
    public void displayDetails() {
        super.displayDetails();
        System.out.println("Contract Amount: $" + contractAmount);
    }
}

// Main Test Class
public class Main {
    public static void main(String[] args) {
        System.out.println("==================================================");
        System.out.println("        EMPLOYEE PAYROLL SYSTEM DEMO              ");
        System.out.println("==================================================");

        // Test Case a: FullTimeEmployee
        FullTimeEmployee emp1 = new FullTimeEmployee("Alice Smith", 101, 5000.0, 1200.0);

        // Test Case b: PartTimeEmployee
        PartTimeEmployee emp2 = new PartTimeEmployee("Bob Jones", 102, 80, 25.0);

        // Test Case c: ContractEmployee
        ContractEmployee emp3 = new ContractEmployee("Charlie Brown", 103, 4000.0);

        // Test Case d: Store using Employee references & execute calculateSalary() polymorphically
        ArrayList<Employee> employees = new ArrayList<>();
        employees.add(emp1);
        employees.add(emp2);
        employees.add(emp3);

        System.out.println("\n--- Processing Payroll (Polymorphism) ---");
        for (Employee e : employees) {
            e.displayDetails();
        }

        // Test Case e & f: Demonstrating Overloaded calculateBonus() methods
        System.out.println("\n==================================================");
        System.out.println("          DEMONSTRATING OVERLOADED BONUS          ");
        System.out.println("==================================================");

        // e. Fixed Bonus
        double fixedBonus = emp1.calculateBonus(500.0);
        System.out.printf("%s Fixed Bonus ($500 flat):$%.2f%n", emp1.getName(), fixedBonus);

        // f. Percentage-based Bonus
        double percentBonus = emp1.calculateBonus(10); // 10% of total salary
        System.out.printf("%s Percentage Bonus (10%%): $%.2f%n", emp1.getName(), percentBonus);

        // f. Performance-based Bonus
        double performanceBonus = emp1.calculateBonus(10, 1.25); // 10% with 1.25 rating multiplier
        System.out.printf("%s Performance Bonus (10%% * 1.25 rating): $%.2f%n", emp1.getName(), performanceBonus);
    }
}
