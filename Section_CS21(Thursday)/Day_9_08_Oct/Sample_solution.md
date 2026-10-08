


**Custom exceptions** (all extend a common checked `ValidationException`):

| Exception | Thrown when |
|---|---|
| `InvalidEmailException` | Email ID breaks rule (i) |
| `InvalidPinCodeException` | PIN is not a 6-digit number |
| `InvalidRollNumberException` | Roll number is not `stud` + 5 digits |
| `InvalidEmployeeIdException` | Employee ID is not `emp` + 3 digits |
| `InvalidNameException` *(extra)* | First/last name contains non-letters |
| `InvalidPanException` *(extra)* | PAN is not in the format `ABCDE1234F` |

The two extra exceptions are included because the assignment asks to validate *all* fields, including name and PAN.

# Validation rules

| Field | Rule | Valid | Invalid |
|---|---|---|---|
| First / last name | letters only | `Rahul` | `Ravi1` |
| PAN | 5 capital letters, 4 digits, 1 capital letter | `ABCDE1234F` | `abcde1234f` |
| PIN code | exactly 6 digits | `781015` | `78101` |
| Email ID | exactly one `@`; part before `@` has letters/digits **and** at least one of `! # $ & *`; valid domain after `@` | `rahul#das@iiitg.ac.in` | `amitroy@gmail.com`, `amit#roy@gmail` |
| Roll number | `stud` followed by exactly 5 digits | `stud12345` | `st12345` |
| Employee ID | `emp` followed by exactly 3 digits | `emp101` | `emp12` |

**Note on the email rule:** the assignment's special-character set is {@, !, #, $, &, \*}. Since an email can contain only one `@` (the separator), the part before `@` must contain at least one of `! # $ & *`.

# Pseudocode

## Custom exceptions

```
CLASS ValidationException EXTENDS Exception
    CONSTRUCTOR(message): call parent with message

CLASS InvalidEmailException       EXTENDS ValidationException
CLASS InvalidPinCodeException     EXTENDS ValidationException
CLASS InvalidRollNumberException  EXTENDS ValidationException
CLASS InvalidEmployeeIdException  EXTENDS ValidationException
CLASS InvalidNameException        EXTENDS ValidationException
CLASS InvalidPanException         EXTENDS ValidationException
```

## Validation helpers

```
FUNCTION validateName(name, label)
    IF name is empty OR name contains anything other than letters THEN
        THROW InvalidNameException(label + " must contain only letters")

FUNCTION validatePan(pan)
    IF pan does not match [5 capital letters][4 digits][1 capital letter] THEN
        THROW InvalidPanException("PAN must be like ABCDE1234F")

FUNCTION validatePin(pin)
    IF length(pin) != 6 OR pin is not all digits THEN
        THROW InvalidPinCodeException("PIN must be a 6-digit number")

FUNCTION validateEmail(email)
    IF email is empty THEN
        THROW InvalidEmailException("Email cannot be empty")
    IF count of '@' in email != 1 THEN
        THROW InvalidEmailException("Email must contain exactly one '@'")
    local  <- part of email before '@'
    domain <- part of email after '@'
    IF local is empty THEN
        THROW InvalidEmailException("Nothing before '@'")
    hasAlnum   <- FALSE
    hasSpecial <- FALSE
    FOR each character c IN local DO
        IF c is a letter or digit THEN hasAlnum <- TRUE
        ELSE IF c IN {!, #, $, &, *} THEN hasSpecial <- TRUE
        ELSE THROW InvalidEmailException("Illegal character c before '@'")
    IF NOT hasAlnum THEN
        THROW InvalidEmailException("Must be alphanumeric before '@'")
    IF NOT hasSpecial THEN
        THROW InvalidEmailException("Need a special char from {!,#,$,&,*}")
    IF domain does not look like name.tld (e.g. gmail.com, iiitg.ac.in) THEN
        THROW InvalidEmailException("Invalid domain after '@'")

FUNCTION validateRollNumber(roll)
    IF roll does not start with "stud" OR
       the rest is not exactly 5 digits THEN
        THROW InvalidRollNumberException("Roll must be 'stud' + 5 digits")

FUNCTION validateEmployeeId(id)
    IF id does not start with "emp" OR
       the rest is not exactly 3 digits THEN
        THROW InvalidEmployeeIdException("Emp ID must be 'emp' + 3 digits")
```

## Classes

```
CLASS Person
    FIELDS firstName, lastName, pan, pin

    CONSTRUCTOR(firstName, lastName, pan, pin)
        store all fields
        IF the object being created is exactly a Person THEN
            CALL validate()
        // (Student/Employee call validate() themselves after
        //  their own fields are stored)

    METHOD validate()
        validateName(firstName, "First name")
        validateName(lastName, "Last name")
        validatePan(pan)
        validatePin(pin)

CLASS Student EXTENDS Person
    FIELDS email, rollNumber

    CONSTRUCTOR(firstName, lastName, pan, pin, email, rollNumber)
        CALL Person constructor(firstName, lastName, pan, pin)
        store email, rollNumber
        CALL validate()

    METHOD validate()          // overrides Person.validate()
        CALL Person.validate()
        validateEmail(email)
        validateRollNumber(rollNumber)

CLASS Employee EXTENDS Person
    FIELDS email, employeeId

    CONSTRUCTOR(firstName, lastName, pan, pin, email, employeeId)
        CALL Person constructor(firstName, lastName, pan, pin)
        store email, employeeId
        CALL validate()

    METHOD validate()          // overrides Person.validate()
        CALL Person.validate()
        validateEmail(email)
        validateEmployeeId(employeeId)
```

## Main program

```
records <- empty list

LOOP forever
    DISPLAY menu: 1.Add Person 2.Add Student 3.Add Employee
                  4.Display all 5.Exit
    READ choice
    CASE choice OF
        1: createRecord("person")
        2: createRecord("student")
        3: createRecord("employee")
        4: PRINT every object in records
        5: PRINT "Goodbye"; STOP
        otherwise: PRINT "Invalid choice"

PROCEDURE createRecord(type)
    READ firstName, lastName, pan, pin from keyboard
    TRY
        IF type = "student" THEN
            READ email, rollNumber
            obj <- NEW Student(..., email, rollNumber)   // validate() runs here
        ELSE IF type = "employee" THEN
            READ email, employeeId
            obj <- NEW Employee(..., email, employeeId)  // validate() runs here
        ELSE
            obj <- NEW Person(...)                       // validate() runs here
        ADD obj to records
        PRINT "Record created successfully"
    CATCH InvalidEmailException e      : PRINT "EMAIL ERROR: "       + e.message
    CATCH InvalidPinCodeException e    : PRINT "PIN CODE ERROR: "    + e.message
    CATCH InvalidRollNumberException e : PRINT "ROLL NUMBER ERROR: " + e.message
    CATCH InvalidEmployeeIdException e : PRINT "EMPLOYEE ID ERROR: " + e.message
    CATCH InvalidNameException e       : PRINT "NAME ERROR: "        + e.message
    CATCH InvalidPanException e        : PRINT "PAN ERROR: "         + e.message
```

**Why the `Person` constructor checks its own class before calling `validate()`:** in Java, a constructor that calls an overridable method runs the *subclass* version. If `Person`'s constructor always called `validate()`, creating a `Student` would run `Student.validate()` before `email` and `rollNumber` were stored, so the check would see `null` values. Letting each class's own constructor trigger validation after all its fields are set avoids this.

# Java code — `InformationSystem.java`

Save as `InformationSystem.java`, then:

```
javac InformationSystem.java
java InformationSystem
```

```java
/*
 * CS 202 Lab - Assignment 8
 * Information System with inheritance and custom exceptions.
 *
 *            Person  (firstName, lastName, pan, pin)
 *            /     \
 *     Student       Employee
 * (email, rollNo)   (email, empId)
 *
 * validate() is called automatically while an object is being created.
 * Any invalid field throws a field-specific custom exception.
 *
 * Compile : javac InformationSystem.java
 * Run     : java InformationSystem
 */

import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

/* 
 *  CUSTOM EXCEPTIONS
 *  All extend a common checked base class, so callers are forced to
 *  handle them, and each one can still be caught individually.
 */

class ValidationException extends Exception {
    public ValidationException(String message) {
        super(message);
    }
}

class InvalidEmailException extends ValidationException {
    public InvalidEmailException(String message) {
        super(message);
    }
}

class InvalidPinCodeException extends ValidationException {
    public InvalidPinCodeException(String message) {
        super(message);
    }
}

class InvalidRollNumberException extends ValidationException {
    public InvalidRollNumberException(String message) {
        super(message);
    }
}

class InvalidEmployeeIdException extends ValidationException {
    public InvalidEmployeeIdException(String message) {
        super(message);
    }
}

/* Extra exceptions so that *all* fields are validated, as the task asks. */
class InvalidNameException extends ValidationException {
    public InvalidNameException(String message) {
        super(message);
    }
}

class InvalidPanException extends ValidationException {
    public InvalidPanException(String message) {
        super(message);
    }
}

/* 
 *  VALIDATION RULES (kept in one place, reused by every class)
 */

final class Validator {

    private Validator() { }

    // Allowed special characters for the part before '@'.
    // '@' itself is in the assignment's set, but it cannot appear twice in an
    // email, so the local part must contain at least one of: ! # $ & *
    private static final String SPECIALS = "!#$&*";

    /** First / last name: letters only. */
    static void validateName(String name, String label) throws InvalidNameException {
        if (name == null || !name.matches("[A-Za-z]+")) {
            throw new InvalidNameException(label + " '" + name
                    + "' is invalid. It must contain only letters (A-Z, a-z).");
        }
    }

    /** PAN: 5 uppercase letters, 4 digits, 1 uppercase letter (e.g. ABCDE1234F). */
    static void validatePan(String pan) throws InvalidPanException {
        if (pan == null || !pan.matches("[A-Z]{5}[0-9]{4}[A-Z]")) {
            throw new InvalidPanException("PAN '" + pan
                    + "' is invalid. Expected format: ABCDE1234F.");
        }
    }

    /** Rule (ii): PIN code must be exactly a 6-digit number. */
    static void validatePin(String pin) throws InvalidPinCodeException {
        if (pin == null || !pin.matches("[0-9]{6}")) {
            throw new InvalidPinCodeException("PIN code '" + pin
                    + "' is invalid. It must be exactly a 6-digit number.");
        }
    }

    /**
     * Rule (i): email must
     *   - contain exactly one '@'
     *   - have a local part (before '@') made of letters/digits and the
     *     special characters ! # $ & *, with at least one letter/digit
     *     AND at least one special character
     *   - have a domain after '@' such as gmail.com or iiitg.ac.in
     */
    static void validateEmail(String email) throws InvalidEmailException {
        if (email == null || email.isEmpty()) {
            throw new InvalidEmailException("Email ID cannot be empty.");
        }

        int at = email.indexOf('@');
        if (at == -1 || at != email.lastIndexOf('@')) {
            throw new InvalidEmailException("Email ID '" + email
                    + "' must contain exactly one '@'.");
        }

        String local = email.substring(0, at);
        String domain = email.substring(at + 1);

        if (local.isEmpty()) {
            throw new InvalidEmailException("Email ID '" + email
                    + "' has nothing before '@'.");
        }

        boolean hasAlnum = false;
        boolean hasSpecial = false;
        for (char c : local.toCharArray()) {
            if (Character.isLetterOrDigit(c) && c < 128) {
                hasAlnum = true;
            } else if (SPECIALS.indexOf(c) >= 0) {
                hasSpecial = true;
            } else {
                throw new InvalidEmailException("Email ID '" + email
                        + "' contains illegal character '" + c + "' before '@'.");
            }
        }
        if (!hasAlnum) {
            throw new InvalidEmailException("Email ID '" + email
                    + "' must be alphanumeric (letters/digits) before '@'.");
        }
        if (!hasSpecial) {
            throw new InvalidEmailException("Email ID '" + email
                    + "' must contain a special character from {!, #, $, &, *} before '@'.");
        }

        // domain: labels of letters/digits/hyphens separated by dots,
        // ending in a top-level label of 2+ letters (gmail.com, iiitg.ac.in)
        if (!domain.matches("([A-Za-z0-9-]+\\.)+[A-Za-z]{2,}")) {
            throw new InvalidEmailException("Email ID '" + email
                    + "' must have a valid domain after '@' (e.g. gmail.com, iiitg.ac.in).");
        }
    }

    /** Rule (iii): roll number = "stud" followed by exactly 5 digits. */
    static void validateRollNumber(String roll) throws InvalidRollNumberException {
        if (roll == null || !roll.matches("stud[0-9]{5}")) {
            throw new InvalidRollNumberException("Roll number '" + roll
                    + "' is invalid. It must be 'stud' followed by 5 digits (e.g. stud12345).");
        }
    }

    /** Rule (iv): employee ID = "emp" followed by exactly 3 digits. */
    static void validateEmployeeId(String id) throws InvalidEmployeeIdException {
        if (id == null || !id.matches("emp[0-9]{3}")) {
            throw new InvalidEmployeeIdException("Employee ID '" + id
                    + "' is invalid. It must be 'emp' followed by 3 digits (e.g. emp123).");
        }
    }
}

/* 
 *  BASE CLASS : Person
 */

class Person {
    protected String firstName;
    protected String lastName;
    protected String pan;
    protected String pin;

    public Person(String firstName, String lastName, String pan, String pin)
            throws ValidationException {
        this.firstName = firstName;
        this.lastName = lastName;
        this.pan = pan;
        this.pin = pin;

        // Only validate here when a plain Person is being built.
        // For Student/Employee, their own constructor calls validate()
        // after their extra fields are set (calling the overridden
        // validate() now would see those fields as null).
        if (getClass() == Person.class) {
            validate();
        }
    }

    /** Validates all Person fields. */
    public void validate() throws ValidationException {
        Validator.validateName(firstName, "First name");
        Validator.validateName(lastName, "Last name");
        Validator.validatePan(pan);
        Validator.validatePin(pin);
    }

    @Override
    public String toString() {
        return "Person   | Name: " + firstName + " " + lastName
                + " | PAN: " + pan + " | PIN: " + pin;
    }
}

/* 
 *  DERIVED CLASS : Student
 */

final class Student extends Person {
    private String email;
    private String rollNumber;

    public Student(String firstName, String lastName, String pan, String pin,
                   String email, String rollNumber) throws ValidationException {
        super(firstName, lastName, pan, pin);
        this.email = email;
        this.rollNumber = rollNumber;
        validate();                       // validation on object creation
    }

    /** Validates inherited fields first, then Student-specific ones. */
    @Override
    public void validate() throws ValidationException {
        super.validate();
        Validator.validateEmail(email);
        Validator.validateRollNumber(rollNumber);
    }

    @Override
    public String toString() {
        return "Student  | Name: " + firstName + " " + lastName
                + " | PAN: " + pan + " | PIN: " + pin
                + " | Email: " + email + " | Roll: " + rollNumber;
    }
}

/* 
 *  DERIVED CLASS : Employee
 */

final class Employee extends Person {
    private String email;
    private String employeeId;

    public Employee(String firstName, String lastName, String pan, String pin,
                    String email, String employeeId) throws ValidationException {
        super(firstName, lastName, pan, pin);
        this.email = email;
        this.employeeId = employeeId;
        validate();                       // validation on object creation
    }

    /** Validates inherited fields first, then Employee-specific ones. */
    @Override
    public void validate() throws ValidationException {
        super.validate();
        Validator.validateEmail(email);
        Validator.validateEmployeeId(employeeId);
    }

    @Override
    public String toString() {
        return "Employee | Name: " + firstName + " " + lastName
                + " | PAN: " + pan + " | PIN: " + pin
                + " | Email: " + email + " | Emp ID: " + employeeId;
    }
}

/* 
 *  DRIVER : menu-driven, reads input from the keyboard
 */

public class InformationSystem {

    private static final Scanner sc = new Scanner(System.in);
    private static final List<Person> records = new ArrayList<>();

    public static void main(String[] args) {
        System.out.println("Information System");

        while (true) {
            System.out.println();
            System.out.println("1. Add Person");
            System.out.println("2. Add Student");
            System.out.println("3. Add Employee");
            System.out.println("4. Display all records");
            System.out.println("5. Exit");
            System.out.print("Enter choice: ");

            if (!sc.hasNextLine()) {
                break;                     // end of input
            }
            String choice = sc.nextLine().trim();

            switch (choice) {
                case "1": createRecord("person");   break;
                case "2": createRecord("student");  break;
                case "3": createRecord("employee"); break;
                case "4": displayAll();             break;
                case "5":
                    System.out.println("Exiting. Goodbye!");
                    return;
                default:
                    System.out.println("Invalid choice. Enter a number from 1 to 5.");
            }
        }
    }

    private static String read(String prompt) {
        System.out.print(prompt);
        return sc.hasNextLine() ? sc.nextLine().trim() : "";
    }

    /** Reads fields, tries to build the object, and reports the specific error. */
    private static void createRecord(String type) {
        String first = read("First name   : ");
        String last  = read("Last name    : ");
        String pan   = read("PAN          : ");
        String pin   = read("Address PIN  : ");

        try {
            Person p;
            if (type.equals("student")) {
                String email = read("Email ID     : ");
                String roll  = read("Roll number  : ");
                p = new Student(first, last, pan, pin, email, roll);
            } else if (type.equals("employee")) {
                String email = read("Email ID     : ");
                String empId = read("Employee ID  : ");
                p = new Employee(first, last, pan, pin, email, empId);
            } else {
                p = new Person(first, last, pan, pin);
            }
            records.add(p);
            System.out.println(">> Record created successfully.");

        } catch (InvalidEmailException e) {
            System.out.println(">> EMAIL ERROR: " + e.getMessage());
        } catch (InvalidPinCodeException e) {
            System.out.println(">> PIN CODE ERROR: " + e.getMessage());
        } catch (InvalidRollNumberException e) {
            System.out.println(">> ROLL NUMBER ERROR: " + e.getMessage());
        } catch (InvalidEmployeeIdException e) {
            System.out.println(">> EMPLOYEE ID ERROR: " + e.getMessage());
        } catch (InvalidNameException e) {
            System.out.println(">> NAME ERROR: " + e.getMessage());
        } catch (InvalidPanException e) {
            System.out.println(">> PAN ERROR: " + e.getMessage());
        } catch (ValidationException e) {   // safety net for any other rule
            System.out.println(">> VALIDATION ERROR: " + e.getMessage());
        }
    }

    private static void displayAll() {
        if (records.isEmpty()) {
            System.out.println("No records yet.");
            return;
        }
        System.out.println("All Records");
        for (Person p : records) {
            System.out.println(p);
        }
    }
}
```



**Valid Student**

```
Enter choice: 2
First name   : Rahul
Last name    : Das
PAN          : ABCDE1234F
Address PIN  : 781015
Email ID     : rahul#das@iiitg.ac.in
Roll number  : stud12345
>> Record created successfully.
```

**Valid Employee**

```
Enter choice: 3
First name   : Priya
Last name    : Sharma
PAN          : PQRST6789K
Address PIN  : 110001
Email ID     : priya$01@gmail.com
Employee ID  : emp101
>> Record created successfully.
```

**Invalid inputs — each triggers its own exception**

| Field entered | Console output |
|---|---|
| PIN `78101` | `>> PIN CODE ERROR: PIN code '78101' is invalid. It must be exactly a 6-digit number.` |
| Email `amitroy@gmail.com` | `>> EMAIL ERROR: Email ID 'amitroy@gmail.com' must contain a special character from {!, #, $, &, *} before '@'.` |
| Email `amit#roy@gmail` | `>> EMAIL ERROR: Email ID 'amit#roy@gmail' must have a valid domain after '@' (e.g. gmail.com, iiitg.ac.in).` |
| Roll `st12345` | `>> ROLL NUMBER ERROR: Roll number 'st12345' is invalid. It must be 'stud' followed by 5 digits (e.g. stud12345).` |
| Emp ID `emp12` | `>> EMPLOYEE ID ERROR: Employee ID 'emp12' is invalid. It must be 'emp' followed by 3 digits (e.g. emp123).` |
| First name `Ravi1` | `>> NAME ERROR: First name 'Ravi1' is invalid. It must contain only letters (A-Z, a-z).` |
| PAN `abcde1234f` | `>> PAN ERROR: PAN 'abcde1234f' is invalid. Expected format: ABCDE1234F.` |

**Display all records**

```
All Records
Student  | Name: Rahul Das | PAN: ABCDE1234F | PIN: 781015 | Email: rahul#das@iiitg.ac.in | Roll: stud12345
Employee | Name: Priya Sharma | PAN: PQRST6789K | PIN: 110001 | Email: priya$01@gmail.com | Emp ID: emp101
Person   | Name: Ravi Kumar | PAN: XYZAB1111C | PIN: 560001
```

Invalid records are never added to the list, because the exception is thrown inside the constructor, so the object is never created.
