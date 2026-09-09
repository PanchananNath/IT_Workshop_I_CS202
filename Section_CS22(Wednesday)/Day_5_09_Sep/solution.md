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
