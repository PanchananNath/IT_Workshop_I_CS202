# Question 1: LCS with String Handling

## Pseudocode

### Main algorithm

```text
START

Create Scanner object

Read first sentence
Read second sentence

IF first sentence is empty or contains only spaces
    Print error message
    STOP
END IF

IF second sentence is empty or contains only spaces
    Print error message
    STOP
END IF

normalised1 = preprocess(first sentence)
normalised2 = preprocess(second sentence)

lcs = findCharacterLCS(normalised1, normalised2)

Print normalised sentences
Print LCS
Print LCS length

similarity = (2 * LCS length) / (length of normalised1 + length of normalised2) * 100
Print similarity formatted to 2 decimal places

Check whether LCS is palindrome
Count vowels in LCS
Count consonants in LCS

Find first letter in LCS
Find its index in both normalised strings
Use substring() to display the part beginning at that index

Split both normalised strings into words
Find longest common sequence of words
Print word-level LCS

END
```

### Preprocessing

```text
FUNCTION preprocess(text)

    Trim leading and trailing spaces
    Convert text to lower case
    Replace every character except letters, digits and spaces with ""
    Replace multiple spaces with one space
    Trim again

    RETURN processed text

END FUNCTION
```

### Character-level LCS

```text
FUNCTION findCharacterLCS(str1, str2)

    Create 2-D integer table dp of size
        (length of str1 + 1) x (length of str2 + 1)

    FOR i from 1 to length of str1
        FOR j from 1 to length of str2

            IF str1.charAt(i - 1) == str2.charAt(j - 1)
                dp[i][j] = dp[i - 1][j - 1] + 1
            ELSE
                dp[i][j] = maximum of dp[i - 1][j] and dp[i][j - 1]
            END IF

        END FOR
    END FOR

    Create StringBuilder

    Set i = length of str1
    Set j = length of str2

    WHILE i > 0 AND j > 0

        IF characters are equal
            Add character to StringBuilder
            i = i - 1
            j = j - 1
        ELSE IF dp[i - 1][j] >= dp[i][j - 1]
            i = i - 1
        ELSE
            j = j - 1
        END IF

    END WHILE

    Reverse StringBuilder

    RETURN StringBuilder as String

END FUNCTION
```

### Palindrome check

```text
FUNCTION isPalindrome(text)

    reversed = reverse text using StringBuilder.reverse()

    RETURN text.equals(reversed)

END FUNCTION
```

### Vowel and consonant counting

```text
FUNCTION countVowels(text)

    count = 0

    FOR each character in text
        IF character is a letter
            IF "aeiou".indexOf(character) >= 0
                count = count + 1
            END IF
        END IF
    END FOR

    RETURN count

END FUNCTION
```

```text
FUNCTION countConsonants(text)

    count = 0

    FOR each character in text
        IF Character.isLetter(character)
            IF "aeiou".indexOf(character) == -1
                count = count + 1
            END IF
        END IF
    END FOR

    RETURN count

END FUNCTION
```

### Word-level LCS

```text
FUNCTION findWordLCS(words1, words2)

    Create 2-D integer table dp

    FOR i from 1 to number of words1
        FOR j from 1 to number of words2

            IF words1[i - 1].equals(words2[j - 1])
                dp[i][j] = dp[i - 1][j - 1] + 1
            ELSE
                dp[i][j] = maximum of dp[i - 1][j] and dp[i][j - 1]
            END IF

        END FOR
    END FOR

    Backtrack through dp

    Store matching words in reverse order

    Reverse the result

    Return result as an array of words

END FUNCTION
```

## Full code: `LCSAnalyzer.java`

```java
import java.util.Scanner;

public class LCSAnalyzer {

    public static String preprocess(String text) {
        text = text.trim();
        text = text.toLowerCase();
        text = text.replaceAll("[^a-z0-9 ]", "");
        text = text.replaceAll("\\s+", " ");
        return text.trim();
    }

    public static String findCharacterLCS(String str1, String str2) {
        int m = str1.length();
        int n = str2.length();

        int[][] dp = new int[m + 1][n + 1];

        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (str1.charAt(i - 1) == str2.charAt(j - 1)) {
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                } else {
                    dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
                }
            }
        }

        StringBuilder lcs = new StringBuilder();

        int i = m;
        int j = n;

        while (i > 0 && j > 0) {
            if (str1.charAt(i - 1) == str2.charAt(j - 1)) {
                lcs.append(str1.charAt(i - 1));
                i--;
                j--;
            } else if (dp[i - 1][j] >= dp[i][j - 1]) {
                i--;
            } else {
                j--;
            }
        }

        return lcs.reverse().toString();
    }

    public static boolean isPalindrome(String text) {
        String reversed = new StringBuilder(text).reverse().toString();
        return text.equals(reversed);
    }

    public static int countVowels(String text) {
        int count = 0;
        String vowels = "aeiou";

        for (int i = 0; i < text.length(); i++) {
            char ch = text.charAt(i);

            if (Character.isLetter(ch) && vowels.indexOf(ch) >= 0) {
                count++;
            }
        }

        return count;
    }

    public static int countConsonants(String text) {
        int count = 0;
        String vowels = "aeiou";

        for (int i = 0; i < text.length(); i++) {
            char ch = text.charAt(i);

            if (Character.isLetter(ch) && vowels.indexOf(ch) == -1) {
                count++;
            }
        }

        return count;
    }

    public static void displayFirstLetterAnalysis(String lcs, String str1, String str2) {
        int firstLetterIndexInLCS = -1;

        for (int i = 0; i < lcs.length(); i++) {
            if (Character.isLetter(lcs.charAt(i))) {
                firstLetterIndexInLCS = i;
                break;
            }
        }

        if (firstLetterIndexInLCS == -1) {
            System.out.println("First letter analysis: No letter found in LCS.");
            return;
        }

        char firstLetter = lcs.charAt(firstLetterIndexInLCS);

        int index1 = str1.indexOf(firstLetter);
        int index2 = str2.indexOf(firstLetter);

        System.out.println("First letter in LCS : " + firstLetter);
        System.out.println("Index in string 1 : " + index1);
        System.out.println("Index in string 2 : " + index2);

        if (index1 >= 0) {
            System.out.println("Substring 1 : " + str1.substring(index1));
        }

        if (index2 >= 0) {
            System.out.println("Substring 2 : " + str2.substring(index2));
        }
    }

    public static String findWordLCS(String[] words1, String[] words2) {
        int m = words1.length;
        int n = words2.length;

        int[][] dp = new int[m + 1][n + 1];

        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (words1[i - 1].equals(words2[j - 1])) {
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                } else {
                    dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
                }
            }
        }

        StringBuilder result = new StringBuilder();

        int i = m;
        int j = n;

        while (i > 0 && j > 0) {
            if (words1[i - 1].equals(words2[j - 1])) {
                if (result.length() > 0) {
                    result.insert(0, " ");
                }

                result.insert(0, words1[i - 1]);
                i--;
                j--;
            } else if (dp[i - 1][j] >= dp[i][j - 1]) {
                i--;
            } else {
                j--;
            }
        }

        return result.toString();
    }

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        System.out.print("Enter first sentence : ");
        String sentence1 = scanner.nextLine();

        System.out.print("Enter second sentence: ");
        String sentence2 = scanner.nextLine();

        if (sentence1.trim().isEmpty()) {
            System.out.println("Error: First sentence cannot be empty.");
            scanner.close();
            return;
        }

        if (sentence2.trim().isEmpty()) {
            System.out.println("Error: Second sentence cannot be empty.");
            scanner.close();
            return;
        }

        String normalized1 = preprocess(sentence1);
        String normalized2 = preprocess(sentence2);

        System.out.println("Normalised 1: " + normalized1);
        System.out.println("Normalised 2: " + normalized2);

        String lcs = findCharacterLCS(normalized1, normalized2);

        System.out.println("LCS (characters) : \"" + lcs + "\"");
        System.out.println("LCS length : " + lcs.length());

        double similarity;

        if (normalized1.length() + normalized2.length() == 0) {
            similarity = 0.0;
        } else {
            similarity = (2.0 * lcs.length())
                    / (normalized1.length() + normalized2.length()) * 100.0;
        }

        System.out.println(
                "Similarity : " + String.format("%.2f%%", similarity)
        );

        System.out.println("Palindrome? : "
                + (isPalindrome(lcs) ? "Yes" : "No"));

        System.out.println("Vowels : " + countVowels(lcs));
        System.out.println("Consonants : " + countConsonants(lcs));

        displayFirstLetterAnalysis(lcs, normalized1, normalized2);

        String[] words1 = normalized1.split(" ");
        String[] words2 = normalized2.split(" ");

        String wordLCS = findWordLCS(words1, words2);

        System.out.println("Longest common word sequence: \""
                + wordLCS + "\"");

        scanner.close();
    }
}
```

---

# Question 2: Object Sorting and the `final` Keyword

## Pseudocode

### Person class

```text
START Person class

Declare protected final String name

Constructor:
    Assign name

final getCompany():
    Return company name

END Person class
```

### Employee class

```text
START Employee class

Declare Employee as final
Extend Person

Declare private final:
    empId
    department
    salary
    joiningYear

Declare public static final:
    COMPANY
    BONUS_RATE

Constructor:
    Call parent constructor
    Assign all final fields

calculateBonus():
    Declare final local variable bonus
    bonus = salary * BONUS_RATE
    Return bonus

compareTo():
    Compare employee IDs
    Return comparison result

toString():
    Return formatted employee details

END Employee class
```

### Employee sorting

```text
START

Create at least 6 Employee objects
Make at least 2 employees have the same salary

Store employees in a final List<Employee>

Print original/company information

Sort using Comparable:
    Collections.sort(list)
    Print list

Sort by salary:
    Use Comparator.comparingDouble(Employee::getSalary).reversed()
    Print list

Sort by department, salary descending, and name:
    Compare department
    then compare salary in reverse order
    then compare name
    Print list

Sort by joining year:
    Create anonymous Comparator<Employee>
    Compare joining years
    Print list

Convert list to Employee array

Call selection sort:
    sortEmployees(array, lambda comparator)
    Print array

END
```

### Manual selection sort

```text
FUNCTION sortEmployees(arr, final Comparator cmp)

    FOR i from 0 to arr.length - 2

        minIndex = i

        FOR j from i + 1 to arr.length - 1

            IF cmp.compare(arr[j], arr[minIndex]) < 0
                minIndex = j
            END IF

        END FOR

        Swap arr[i] and arr[minIndex]

    END FOR

END FUNCTION
```

## Full code: `Person.java`

```java
public class Person {
    protected final String name;

    public Person(String name) {
        this.name = name;
    }

    public final String getCompany() {
        return Employee.COMPANY;
    }

    public String getName() {
        return name;
    }
}
```

## Full code: `Employee.java`

```java
public final class Employee extends Person implements Comparable<Employee> {

    public static final String COMPANY = "IIT Bongora Private Limited.";
    public static final double BONUS_RATE = 0.10;

    private final int empId;
    private final String department;
    private final double salary;
    private final int joiningYear;

    public Employee(int empId, String name, String department,
                    double salary, int joiningYear) {
        super(name);
        this.empId = empId;
        this.department = department;
        this.salary = salary;
        this.joiningYear = joiningYear;
    }

    public int getEmpId() {
        return empId;
    }

    public String getDepartment() {
        return department;
    }

    public double getSalary() {
        return salary;
    }

    public int getJoiningYear() {
        return joiningYear;
    }

    public double calculateBonus() {
        final double bonus = salary * BONUS_RATE;
        return bonus;
    }

    @Override
    public int compareTo(Employee other) {
        return Integer.compare(this.empId, other.empId);
    }

    @Override
    public String toString() {
        return String.format(
                "%-4d %-15s %-10s %10.2f %6d %10.2f",
                empId,
                name,
                department,
                salary,
                joiningYear,
                calculateBonus()
        );
    }
}
```

## Full code: `EmployeeSorting.java`

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Collections;
import java.util.Comparator;
import java.util.List;

public class EmployeeSorting {

    public static void printEmployees(List<Employee> employees) {
        System.out.printf(
                "%-4s %-15s %-10s %10s %6s %10s%n",
                "ID", "Name", "Dept", "Salary", "Year", "Bonus"
        );

        for (Employee employee : employees) {
            System.out.println(employee);
        }

        System.out.println();
    }

    public static void printEmployees(Employee[] employees) {
        System.out.printf(
                "%-4s %-15s %-10s %10s %6s %10s%n",
                "ID", "Name", "Dept", "Salary", "Year", "Bonus"
        );

        for (Employee employee : employees) {
            System.out.println(employee);
        }

        System.out.println();
    }

    public static void sortEmployees(
            Employee[] arr,
            final Comparator<Employee> cmp) {

        for (int i = 0; i < arr.length - 1; i++) {
            int minIndex = i;

            for (int j = i + 1; j < arr.length; j++) {
                if (cmp.compare(arr[j], arr[minIndex]) < 0) {
                    minIndex = j;
                }
            }

            if (minIndex != i) {
                Employee temp = arr[i];
                arr[i] = arr[minIndex];
                arr[minIndex] = temp;
            }
        }
    }

    public static void main(String[] args) {

        final List<Employee> employees = new ArrayList<>();

        employees.add(new Employee(
                101, "Ananya", "HR", 48000.00, 2021
        ));

        employees.add(new Employee(
                102, "Ishita", "Finance", 61000.00, 2020
        ));

        employees.add(new Employee(
                108, "Kabir", "IT", 72000.00, 2016
        ));

        employees.add(new Employee(
                105, "Riya", "IT", 72000.00, 2019
        ));

        employees.add(new Employee(
                103, "Rahul", "Finance", 55000.00, 2018
        ));

        employees.add(new Employee(
                110, "Zoya", "HR", 48000.00, 2022
        ));

        System.out.println("Company: " + Employee.COMPANY);
        System.out.println();

        System.out.println("Sorted by Employee ID (Comparable):");

        Collections.sort(employees);
        printEmployees(employees);

        System.out.println("Sorted by Salary (descending):");

        employees.sort(
                Comparator.comparingDouble(Employee::getSalary).reversed()
        );
        printEmployees(employees);

        System.out.println(
                "Sorted by Department, then Salary (desc), then Name:"
        );

        employees.sort(
                Comparator.comparing(Employee::getDepartment)
                        .thenComparing(
                                Comparator.comparingDouble(Employee::getSalary)
                                        .reversed()
                        )
                        .thenComparing(Employee::getName)
        );
        printEmployees(employees);

        System.out.println("Sorted by Joining Year (anonymous class):");

        employees.sort(new Comparator<Employee>() {
            @Override
            public int compare(Employee e1, Employee e2) {
                return Integer.compare(
                        e1.getJoiningYear(),
                        e2.getJoiningYear()
                );
            }
        });

        printEmployees(employees);

        System.out.println("Sorted by Name using manual selection sort:");

        Employee[] employeeArray = employees.toArray(new Employee[0]);

        sortEmployees(
                employeeArray,
                (e1, e2) -> e1.getName().compareTo(e2.getName())
        );

        printEmployees(employeeArray);
    }
}
```

---

# Question 3: String Pattern Matching and Custom Exceptions

## Pseudocode

### Validation exception hierarchy

```text
START

Create ValidationException extending Exception

Create:
    InvalidEmailException
    InvalidPinCodeException
    InvalidRollNumberException
    InvalidEmployeeIdException

Each subclass extends ValidationException

END
```

### Email validation

```text
FUNCTION validateEmail(email)

    Create regular expression for:
        alphanumeric first part
        one special character from ! # $ & *
        optional alphanumeric characters
        @
        domain name
        one or more domain extensions

    Use matches() to test email

    IF email does not match
        THROW InvalidEmailException
    END IF

END FUNCTION
```

### PIN validation

```text
FUNCTION validatePinCode(pin)

    Pattern = exactly 6 digits

    IF pin does not match pattern
        THROW InvalidPinCodeException
    END IF

END FUNCTION
```

### Roll number validation

```text
FUNCTION validateRollNumber(rollNumber)

    Pattern = stud followed by exactly 5 digits

    IF roll number does not match
        THROW InvalidRollNumberException
    END IF

END FUNCTION
```

### Employee ID validation

```text
FUNCTION validateEmployeeId(employeeId)

    Pattern = emp followed by exactly 3 digits

    IF employee ID does not match
        THROW InvalidEmployeeIdException
    END IF

END FUNCTION
```

### Main algorithm

```text
START

Create Scanner

Read email
Read PIN
Read roll number
Read employee ID

TRY
    validate email
    Print "Email ID is valid."
CATCH InvalidEmailException
    Print exception message
END TRY

TRY
    validate PIN
    Print "PIN code is valid."
CATCH InvalidPinCodeException
    Print exception message
END TRY

TRY
    validate roll number
    Print "Roll number is valid."
CATCH InvalidRollNumberException
    Print exception message
END TRY

TRY
    validate employee ID
    Print "Employee id is valid."
CATCH InvalidEmployeeIdException
    Print exception message
END TRY

Close Scanner

END
```

## Full code: `ValidationException.java`

```java
public class ValidationException extends Exception {

    public ValidationException(String message) {
        super(message);
    }
}
```

## Full code: `InvalidEmailException.java`

```java
public class InvalidEmailException extends ValidationException {

    public InvalidEmailException(String message) {
        super(message);
    }
}
```

## Full code: `InvalidPinCodeException.java`

```java
public class InvalidPinCodeException extends ValidationException {

    public InvalidPinCodeException(String message) {
        super(message);
    }
}
```

## Full code: `InvalidRollNumberException.java`

```java
public class InvalidRollNumberException extends ValidationException {

    public InvalidRollNumberException(String message) {
        super(message);
    }
}
```

## Full code: `InvalidEmployeeIdException.java`

```java
public class InvalidEmployeeIdException extends ValidationException {

    public InvalidEmployeeIdException(String message) {
        super(message);
    }
}
```

## Full code: `Validator.java`

```java
import java.util.regex.Pattern;

public class Validator {

    public static void validateEmail(String email)
            throws InvalidEmailException {

        /*
         * Assumption from the assignment:
         * '@' is the separator, so the special character required
         * before '@' is one of ! # $ & *.
         *
         * The local part contains letters/digits plus at least one
         * required special character.
         */
        String emailRegex =
                "^[A-Za-z0-9]+[!#$&*][A-Za-z0-9]*@"
                        + "[A-Za-z0-9]+(\\.[A-Za-z0-9]+)+$";

        if (!Pattern.matches(emailRegex, email)) {
            throw new InvalidEmailException(
                    "Invalid Email ID: " + email
                            + " (must contain ! # $ & * before @)"
            );
        }
    }

    public static void validatePinCode(String pin)
            throws InvalidPinCodeException {

        String pinRegex = "^\\d{6}$";

        if (!Pattern.matches(pinRegex, pin)) {
            throw new InvalidPinCodeException(
                    "Invalid PIN code: " + pin
                            + " (must be exactly 6 digits)"
            );
        }
    }

    public static void validateRollNumber(String rollNumber)
            throws InvalidRollNumberException {

        String rollRegex = "^stud\\d{5}$";

        if (!Pattern.matches(rollRegex, rollNumber)) {
            throw new InvalidRollNumberException(
                    "Invalid Roll number: " + rollNumber
                            + " must start with 'stud' followed by 5 digits."
            );
        }
    }

    public static void validateEmployeeId(String employeeId)
            throws InvalidEmployeeIdException {

        String employeeRegex = "^emp\\d{3}$";

        if (!Pattern.matches(employeeRegex, employeeId)) {
            throw new InvalidEmployeeIdException(
                    "Invalid Employee id: " + employeeId
                            + " must start with 'emp' followed by 3 digits."
            );
        }
    }
}
```

## Full code: `ValidationDemo.java`

```java
import java.util.Scanner;

public class ValidationDemo {

    public static void main(String[] args) {

        Scanner scanner = new Scanner(System.in);

        System.out.print("Enter Email ID : ");
        String email = scanner.nextLine();

        System.out.print("Enter PIN code : ");
        String pin = scanner.nextLine();

        System.out.print("Enter Roll number : ");
        String rollNumber = scanner.nextLine();

        System.out.print("Enter Employee id : ");
        String employeeId = scanner.nextLine();

        try {
            Validator.validateEmail(email);
            System.out.println("Email ID is valid.");
        } catch (InvalidEmailException e) {
            System.out.println("EmailException -> " + e.getMessage());
        }

        try {
            Validator.validatePinCode(pin);
            System.out.println("PIN code is valid.");
        } catch (InvalidPinCodeException e) {
            System.out.println("PINException -> " + e.getMessage());
        }

        try {
            Validator.validateRollNumber(rollNumber);
            System.out.println("Roll number is valid.");
        } catch (InvalidRollNumberException e) {
            System.out.println("RollNoException -> " + e.getMessage());
        }

        try {
            Validator.validateEmployeeId(employeeId);
            System.out.println("Employee id is valid.");
        } catch (InvalidEmployeeIdException e) {
            System.out.println("EmployeeIdException -> " + e.getMessage());
        }

        scanner.close();
    }
}
```

Note: The `RollNoException` section above should use the following corrected block:

```java
try {
    Validator.validateRollNumber(rollNumber);
    System.out.println("Roll number is valid.");
} catch (InvalidRollNumberException e) {
    System.out.println("RollNoException -> " + e.getMessage());
}
```

---

# File Structure

Use the following files if submitting each class separately:

```text
Assignment7/
|
|-- LCSAnalyzer.java
|
|-- Person.java
|-- Employee.java
|-- EmployeeSorting.java
|
|-- ValidationException.java
|-- InvalidEmailException.java
|-- InvalidPinCodeException.java
|-- InvalidRollNumberException.java
|-- InvalidEmployeeIdException.java
|-- Validator.java
|-- ValidationDemo.java
|
`-- README.md
```

# Compilation and Execution

## Question 1

```bash
javac LCSAnalyzer.java
java LCSAnalyzer
```

## Question 2

```bash
javac Person.java Employee.java EmployeeSorting.java
java EmployeeSorting
```

## Question 3

```bash
javac ValidationException.java InvalidEmailException.java InvalidPinCodeException.java InvalidRollNumberException.java InvalidEmployeeIdException.java Validator.java ValidationDemo.java
java ValidationDemo
```

# Important Note

For Question 3, the assignment says the email must contain a special character before `@`, and specifically notes that `@` itself is the separator. This implementation therefore treats `!`, `#`, `$`, `&`, and `*` as the required special characters in the part before `@`. 

