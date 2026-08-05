# Java Programming Lab 1 — Sample Solutions (Introduction to Java: Basics, Operators, Control Flow & Loops)



> All programs use `Scanner` to take inputs from the user at runtime. Each program should be saved as `<ClassName>.java`, where `<ClassName>` matches the public class name exactly. Sample inputs are given for each program so you can test them against the expected output shown.

---

## Q1. Simple Calculator

**File name:** `Calculator.java`

```java
import java.util.Scanner;

public class Calculator {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        System.out.print("Enter first number: ");
        int a = sc.nextInt();
        System.out.print("Enter second number: ");
        int b = sc.nextInt();

        System.out.println("Sum: " + (a + b));
        System.out.println("Difference: " + (a - b));
        System.out.println("Product: " + (a * b));
        System.out.println("Quotient: " + (a / b));
        System.out.println("Remainder: " + (a % b));

        sc.close();
    }
}
```

**Sample Input:** `15`, `4`

**Expected Output:**
```
Enter first number: Enter second number: Sum: 19
Difference: 11
Product: 60
Quotient: 3
Remainder: 3
```

---

## Q2. Area and Perimeter of a Rectangle

**File name:** `RectangleArea.java`

```java
import java.util.Scanner;

public class RectangleArea {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        System.out.print("Enter length: ");
        double length = sc.nextDouble();
        System.out.print("Enter breadth: ");
        double breadth = sc.nextDouble();

        double area = length * breadth;
        double perimeter = 2 * (length + breadth);

        System.out.println("Area of Rectangle: " + area);
        System.out.println("Perimeter of Rectangle: " + perimeter);

        sc.close();
    }
}
```

**Sample Input:** `12.5`, `6.0`

**Expected Output:**
```
Enter length: Enter breadth: Area of Rectangle: 75.0
Perimeter of Rectangle: 37.0
```

---

## Q3. Area and Circumference of a Circle

**File name:** `CircleArea.java`

```java
import java.util.Scanner;

public class CircleArea {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        System.out.print("Enter radius: ");
        double radius = sc.nextDouble();
        double pi = 3.14159;

        double area = pi * radius * radius;
        double circumference = 2 * pi * radius;

        System.out.println("Area of Circle: " + area);
        System.out.println("Circumference of Circle: " + circumference);

        sc.close();
    }
}
```

**Sample Input:** `7.0`

**Expected Output:**
```
Enter radius: Area of Circle: 153.93791
Circumference of Circle: 43.98226
```

---

## Q4. Prime Number Check

**File name:** `PrimeCheck.java`

```java
import java.util.Scanner;

public class PrimeCheck {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        System.out.print("Enter a number: ");
        int number = sc.nextInt();
        boolean isPrime = true;

        if (number <= 1) {
            isPrime = false;
        } else {
            for (int i = 2; i <= Math.sqrt(number); i++) {
                if (number % i == 0) {
                    isPrime = false;
                    break;
                }
            }
        }

        if (isPrime) {
            System.out.println(number + " is a Prime number.");
        } else {
            System.out.println(number + " is NOT a Prime number.");
        }

        sc.close();
    }
}
```

**Sample Input:** `29`

**Expected Output:**
```
Enter a number: 29 is a Prime number.
```

---

## Q5. Even or Odd Number Check

**File name:** `EvenOddCheck.java`

```java
import java.util.Scanner;

public class EvenOddCheck {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        System.out.print("Enter a number: ");
        int number = sc.nextInt();

        if (number % 2 == 0) {
            System.out.println(number + " is an Even number.");
        } else {
            System.out.println(number + " is an Odd number.");
        }

        sc.close();
    }
}
```

**Sample Input:** `18`

**Expected Output:**
```
Enter a number: 18 is an Even number.
```

---

## Q6. Largest of Three Numbers

**File name:** `LargestOfThree.java`

```java
import java.util.Scanner;

public class LargestOfThree {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        System.out.print("Enter first number: ");
        int a = sc.nextInt();
        System.out.print("Enter second number: ");
        int b = sc.nextInt();
        System.out.print("Enter third number: ");
        int c = sc.nextInt();

        int largest;
        if (a >= b && a >= c) {
            largest = a;
        } else if (b >= a && b >= c) {
            largest = b;
        } else {
            largest = c;
        }

        System.out.println("Largest number: " + largest);

        sc.close();
    }
}
```

**Sample Input:** `23`, `45`, `10`

**Expected Output:**
```
Enter first number: Enter second number: Enter third number: Largest number: 45
```

---

## Q7. Leap Year Check

**File name:** `LeapYearCheck.java`

```java
import java.util.Scanner;

public class LeapYearCheck {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        System.out.print("Enter a year: ");
        int year = sc.nextInt();
        boolean isLeap;

        if (year % 4 == 0 && (year % 100 != 0 || year % 400 == 0)) {
            isLeap = true;
        } else {
            isLeap = false;
        }

        if (isLeap) {
            System.out.println(year + " is a Leap year.");
        } else {
            System.out.println(year + " is NOT a Leap year.");
        }

        sc.close();
    }
}
```

**Sample Input:** `2024`

**Expected Output:**
```
Enter a year: 2024 is a Leap year.
```

---

## Q8. Simple Interest Calculator

**File name:** `SimpleInterest.java`

```java
import java.util.Scanner;

public class SimpleInterest {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        System.out.print("Enter principal amount: ");
        double principal = sc.nextDouble();
        System.out.print("Enter rate of interest: ");
        double rate = sc.nextDouble();
        System.out.print("Enter time period (years): ");
        double time = sc.nextDouble();

        double simpleInterest = (principal * rate * time) / 100;
        double totalAmount = principal + simpleInterest;

        System.out.println("Simple Interest: " + simpleInterest);
        System.out.println("Total Amount: " + totalAmount);

        sc.close();
    }
}
```

**Sample Input:** `10000.0`, `7.5`, `3`

**Expected Output:**
```
Enter principal amount: Enter rate of interest: Enter time period (years): Simple Interest: 2250.0
Total Amount: 12250.0
```

---

## Q9. Swap Two Numbers Without Using a Third Variable

**File name:** `SwapNumbers.java`

```java
import java.util.Scanner;

public class SwapNumbers {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        System.out.print("Enter first number: ");
        int a = sc.nextInt();
        System.out.print("Enter second number: ");
        int b = sc.nextInt();

        System.out.println("Before Swapping: a = " + a + ", b = " + b);

        a = a + b;
        b = a - b;
        a = a - b;

        System.out.println("After Swapping: a = " + a + ", b = " + b);

        sc.close();
    }
}
```

**Sample Input:** `5`, `9`

**Expected Output:**
```
Enter first number: Enter second number: Before Swapping: a = 5, b = 9
After Swapping: a = 9, b = 5
```

*Alternative approach using the XOR bitwise operator:*
```java
a = a ^ b;
b = a ^ b;
a = a ^ b;
```

---

## Q10. Day of the Week Using switch-case

**File name:** `DayOfWeek.java`

```java
import java.util.Scanner;

public class DayOfWeek {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        System.out.print("Enter day number (1-7): ");
        int day = sc.nextInt();
        String dayName;

        switch (day) {
            case 1:
                dayName = "Monday";
                break;
            case 2:
                dayName = "Tuesday";
                break;
            case 3:
                dayName = "Wednesday";
                break;
            case 4:
                dayName = "Thursday";
                break;
            case 5:
                dayName = "Friday";
                break;
            case 6:
                dayName = "Saturday";
                break;
            case 7:
                dayName = "Sunday";
                break;
            default:
                dayName = "Invalid day number";
        }

        System.out.println("Day name: " + dayName);

        sc.close();
    }
}
```

**Sample Input:** `4`

**Expected Output:**
```
Enter day number (1-7): Day name: Thursday
```

---

## Q11. Binary to Decimal and Decimal to Binary Conversion

**File name:** `BinaryDecimalConversion.java`

```java
import java.util.Scanner;

public class BinaryDecimalConversion {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        // Binary to Decimal
        System.out.print("Enter a binary number: ");
        String binaryInput = sc.next();
        int decimalResult = Integer.parseInt(binaryInput, 2);
        System.out.println("Binary " + binaryInput + " in Decimal is: " + decimalResult);

        // Decimal to Binary
        System.out.print("Enter a decimal number: ");
        int decimalInput = sc.nextInt();
        String binaryResult = Integer.toBinaryString(decimalInput);
        System.out.println("Decimal " + decimalInput + " in Binary is: " + binaryResult);

        sc.close();
    }
}
```

**Sample Input:** `1011`, `25`

**Expected Output:**
```
Enter a binary number: Binary 1011 in Decimal is: 11
Enter a decimal number: Decimal 25 in Binary is: 11001
```

**Notes:**
- `Integer.parseInt(binaryInput, 2)` parses a string as a base-2 (binary) number and returns its decimal value.
- `Integer.toBinaryString(decimalInput)` converts an `int` decimal value into its binary string representation.
- The program assumes valid binary digits (0s and 1s) are entered; invalid input will throw a `NumberFormatException`.

---

## Compilation & Execution Notes

For each program:

```bash
javac <ClassName>.java
java <ClassName>
```

Example, for Q1 (input values are typed at the prompts when the program runs):
```bash
javac Calculator.java
java Calculator
```

