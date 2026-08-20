# Assignment 3 — IT Workshop I



---

## Question 1: Count Odd and Even Numbers

### Logic

1. Read the number of test cases, `n`.
2. Read an array of `n` integers where each integer represents the number of elements in the corresponding test case.
3. For each test case:
   - Read the specified number of values.
   - Check every value.
   - If the value is divisible by 2, increment the even count.
   - Otherwise, increment the odd count.
4. Display the odd and even count for each test case.
5. Maintain overall odd and even counters and display their totals after all test cases are processed.

The assignment specifies that the test cases can have different lengths. fileciteturn0file0L4-L14

### Pseudocode

```text
START

READ n

CREATE an integer array size[n]

FOR i = 0 TO n - 1
    READ size[i]
END FOR

overallOdd = 0
overallEven = 0

FOR testCase = 0 TO n - 1

    odd = 0
    even = 0

    FOR j = 0 TO size[testCase] - 1
        READ value

        IF value MOD 2 == 0 THEN
            even = even + 1
            overallEven = overallEven + 1
        ELSE
            odd = odd + 1
            overallOdd = overallOdd + 1
        END IF
    END FOR

    PRINT odd and even count for this test case

END FOR

PRINT overallOdd
PRINT overallEven

END
```

### Final Java Code

```java
import java.util.Scanner;

public class OddEvenTestCases {

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        // Input 1: Number of test cases
        System.out.print("Enter number of test cases: ");
        int n = sc.nextInt();

        // Input 2: Number of elements in each test case
        int[] size = new int[n];

        System.out.println("Enter the number of elements for each test case:");
        for (int i = 0; i < n; i++) {
            System.out.print("Test case " + (i + 1) + ": ");
            size[i] = sc.nextInt();
        }

        int overallOdd = 0;
        int overallEven = 0;

        // Input 3: Values of all test cases
        for (int i = 0; i < n; i++) {
            int odd = 0;
            int even = 0;

            System.out.println("Enter " + size[i]
                    + " values for test case " + (i + 1) + ":");

            for (int j = 0; j < size[i]; j++) {
                int value = sc.nextInt();

                if (value % 2 == 0) {
                    even++;
                    overallEven++;
                } else {
                    odd++;
                    overallOdd++;
                }
            }

            System.out.println("Test Case " + (i + 1));
            System.out.println("Odd numbers  : " + odd);
            System.out.println("Even numbers : " + even);
        }

        System.out.println("\nOverall Count");
        System.out.println("Total odd numbers  : " + overallOdd);
        System.out.println("Total even numbers : " + overallEven);

        sc.close();
    }
}
```

---

## Question 2: Library Management System

The assignment defines a `Book` class with book ID, title, publication year, author, publisher, available copies, and total copies, along with constructors and `setDetails`, `getDetails`, `issue`, and `return` operations. It also requires an array of at least five `Book` objects and a menu-driven interface. fileciteturn0file0L19-L43

### Logic

#### Book Class

1. Create the required instance variables:
   - `bookId`
   - `bookTitle`
   - `yearOfPublication`
   - `authorName`
   - `publisherName`
   - `numberOfAvailableCopies`
   - `totalCopies`
2. Provide:
   - A default constructor.
   - A constructor accepting `totalCopies`.
3. `setDetails()` accepts book information and initializes the object.
4. `getDetails(id)` searches for the book by ID and displays its information.
5. `issue(id)`:
   - Find the book by ID.
   - If an available copy exists, decrease available copies by 1.
   - Otherwise display that no copy is available.
6. `returnBook(id)`:
   - Find the book by ID.
   - If the number of available copies is less than total copies, increase available copies by 1.
   - Otherwise display that all copies are already available.

> Java cannot define a method named `return` because `return` is a reserved Java keyword. Therefore, the implementation uses `returnBook(int id)` for the assignment's required return operation.

#### Menu

1. Create an array containing at least five `Book` objects.
2. Set initial details for all five books.
3. Display a menu repeatedly:
   - `1` — Set Details
   - `2` — Get Details
   - `3` — Issue
   - `4` — Return
   - `5` — Exit
4. Ask for the book ID where required.
5. Search the object array for that ID.
6. Call the corresponding method.
7. Continue until the user selects Exit.

### Pseudocode

```text
CLASS Book

    DECLARE bookId
    DECLARE bookTitle
    DECLARE yearOfPublication
    DECLARE authorName
    DECLARE publisherName
    DECLARE numberOfAvailableCopies
    DECLARE totalCopies

    CONSTRUCTOR Book()
        totalCopies = 0
        numberOfAvailableCopies = 0
    END CONSTRUCTOR

    CONSTRUCTOR Book(totalCopies)
        this.totalCopies = totalCopies
        numberOfAvailableCopies = totalCopies
    END CONSTRUCTOR

    METHOD setDetails(id, title, year, author, publisher, count)
        bookId = id
        bookTitle = title
        yearOfPublication = year
        authorName = author
        publisherName = publisher
        totalCopies = count
        numberOfAvailableCopies = count
    END METHOD

    METHOD getDetails(id)
        IF bookId == id THEN
            DISPLAY all book details
        ELSE
            DISPLAY "Book not found"
        END IF
    END METHOD

    METHOD issue(id)
        IF bookId == id THEN
            IF numberOfAvailableCopies > 0 THEN
                numberOfAvailableCopies = numberOfAvailableCopies - 1
                DISPLAY "Book issued successfully"
            ELSE
                DISPLAY "No copy available"
            END IF
        END IF
    END METHOD

    METHOD returnBook(id)
        IF bookId == id THEN
            IF numberOfAvailableCopies < totalCopies THEN
                numberOfAvailableCopies = numberOfAvailableCopies + 1
                DISPLAY "Book returned successfully"
            ELSE
                DISPLAY "All copies are already available"
            END IF
        END IF
    END METHOD

END CLASS


MAIN

    CREATE Book array of size 5

    CREATE five Book objects
    SET details for each object

    REPEAT

        DISPLAY menu

        READ choice

        SWITCH choice

            CASE 1:
                READ book details
                FIND book by ID
                SET its details
                BREAK

            CASE 2:
                READ book ID
                FIND book by ID
                DISPLAY its details
                BREAK

            CASE 3:
                READ book ID
                FIND book by ID
                ISSUE book
                BREAK

            CASE 4:
                READ book ID
                FIND book by ID
                RETURN book
                BREAK

            CASE 5:
                DISPLAY exit message
                STOP

            DEFAULT:
                DISPLAY invalid choice

        END SWITCH

    UNTIL choice == 5

END
```

### Final Java Code

```java
import java.util.Scanner;

class Book {
    private int bookId;
    private String bookTitle;
    private int yearOfPublication;
    private String authorName;
    private String publisherName;
    private int numberOfAvailableCopies;
    private int totalCopies;

    // Default constructor
    public Book() {
        totalCopies = 0;
        numberOfAvailableCopies = 0;
    }

    // Constructor with total copies
    public Book(int totalCopies) {
        this.totalCopies = totalCopies;
        this.numberOfAvailableCopies = totalCopies;
    }

    // Set details using user input
    public void setDetails(Scanner sc) {
        System.out.print("Enter Book ID: ");
        bookId = sc.nextInt();
        sc.nextLine();

        System.out.print("Enter Book Title: ");
        bookTitle = sc.nextLine();

        System.out.print("Enter Year of Publication: ");
        yearOfPublication = sc.nextInt();
        sc.nextLine();

        System.out.print("Enter Author Name: ");
        authorName = sc.nextLine();

        System.out.print("Enter Publisher Name: ");
        publisherName = sc.nextLine();

        System.out.print("Enter Total Copies: ");
        totalCopies = sc.nextInt();

        numberOfAvailableCopies = totalCopies;
    }

    // Set details using parameters
    public void setDetails(int id, String title, int year,
                           String author, String publisher, int count) {
        bookId = id;
        bookTitle = title;
        yearOfPublication = year;
        authorName = author;
        publisherName = publisher;
        totalCopies = count;
        numberOfAvailableCopies = count;
    }

    // Display details of the book identified by ID
    public boolean getDetails(int id) {
        if (bookId == id) {
            System.out.println("\n--- Book Details ---");
            System.out.println("Book ID                : " + bookId);
            System.out.println("Book Title             : " + bookTitle);
            System.out.println("Year of Publication    : " + yearOfPublication);
            System.out.println("Author Name            : " + authorName);
            System.out.println("Publisher Name         : " + publisherName);
            System.out.println("Total Copies           : " + totalCopies);
            System.out.println("Available Copies       : "
                    + numberOfAvailableCopies);
            return true;
        }

        return false;
    }

    // Issue a book
    public boolean issue(int id) {
        if (bookId == id) {
            if (numberOfAvailableCopies > 0) {
                numberOfAvailableCopies--;
                System.out.println("Book issued successfully.");
            } else {
                System.out.println("No copy of this book is currently available.");
            }
            return true;
        }

        return false;
    }

    // Return a book
    // The assignment calls this operation "return", but return is a
    // reserved keyword in Java, so the method is named returnBook().
    public boolean returnBook(int id) {
        if (bookId == id) {
            if (numberOfAvailableCopies < totalCopies) {
                numberOfAvailableCopies++;
                System.out.println("Book returned successfully.");
            } else {
                System.out.println("All copies are already available.");
            }
            return true;
        }

        return false;
    }
}

public class LibraryManagementSystem {

    // Find a book by its ID
    public static int findBook(Book[] books, int id) {
        for (int i = 0; i < books.length; i++) {
            if (books[i].getDetails(id)) {
                return i;
            }
        }
        return -1;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        // Create an array of five Book objects
        Book[] bookArray = new Book[5];

        for (int i = 0; i < bookArray.length; i++) {
            bookArray[i] = new Book();
        }

        // Set initial details for five books
        bookArray[0].setDetails(
                101,
                "Java Programming",
                2023,
                "Herbert Schildt",
                "McGraw Hill",
                5
        );

        bookArray[1].setDetails(
                102,
                "Data Structures",
                2022,
                "Seymour Lipschutz",
                "McGraw Hill",
                4
        );

        bookArray[2].setDetails(
                103,
                "Operating System Concepts",
                2021,
                "Abraham Silberschatz",
                "Wiley",
                3
        );

        bookArray[3].setDetails(
                104,
                "Computer Networks",
                2020,
                "Andrew S. Tanenbaum",
                "Pearson",
                6
        );

        bookArray[4].setDetails(
                105,
                "Database Management Systems",
                2023,
                "Raghu Ramakrishnan",
                "McGraw Hill",
                5
        );

        int choice;

        do {
            System.out.println("\n========== LIBRARY MANAGEMENT SYSTEM ==========");
            System.out.println("1. Set Details");
            System.out.println("2. Get Details");
            System.out.println("3. Issue");
            System.out.println("4. Return");
            System.out.println("5. Exit");
            System.out.print("Enter your choice: ");

            choice = sc.nextInt();

            switch (choice) {

                case 1:
                    System.out.print("Enter Book ID to set/update: ");
                    int setId = sc.nextInt();

                    int setIndex = -1;

                    for (int i = 0; i < bookArray.length; i++) {
                        // getDetails prints only when ID matches, so use
                        // a separate search helper below.
                        if (bookArray[i].getDetails(setId)) {
                            setIndex = i;
                            break;
                        }
                    }

                    if (setIndex != -1) {
                        System.out.println("Enter new details:");
                        bookArray[setIndex].setDetails(sc);
                    } else {
                        System.out.println(
                                "Book ID not found. Entering details for "
                                + "the first available object is not supported."
                        );
                    }
                    break;

                case 2:
                    System.out.print("Enter Book ID: ");
                    int getId = sc.nextInt();

                    boolean foundForGet = false;

                    for (Book book : bookArray) {
                        if (book.getDetails(getId)) {
                            foundForGet = true;
                            break;
                        }
                    }

                    if (!foundForGet) {
                        System.out.println("Book ID not found.");
                    }
                    break;

                case 3:
                    System.out.print("Enter Book ID to issue: ");
                    int issueId = sc.nextInt();

                    boolean foundForIssue = false;

                    for (Book book : bookArray) {
                        if (book.issue(issueId)) {
                            foundForIssue = true;
                            break;
                        }
                    }

                    if (!foundForIssue) {
                        System.out.println("Book ID not found.");
                    }
                    break;

                case 4:
                    System.out.print("Enter Book ID to return: ");
                    int returnId = sc.nextInt();

                    boolean foundForReturn = false;

                    for (Book book : bookArray) {
                        if (book.returnBook(returnId)) {
                            foundForReturn = true;
                            break;
                        }
                    }

                    if (!foundForReturn) {
                        System.out.println("Book ID not found.");
                    }
                    break;

                case 5:
                    System.out.println("Exiting Library Management System...");
                    break;

                default:
                    System.out.println("Invalid choice. Please try again.");
            }

        } while (choice != 5);

        sc.close();
    }
}
```

### Note on the `Set Details` Option

For a cleaner implementation, the menu can use a separate `findBookIndex()` method rather than calling `getDetails()` while searching, because `getDetails()` also displays the book. The version below is the recommended final implementation for submission if you want cleaner output:

```java
public static int findBookIndex(Book[] books, int id) {
    for (int i = 0; i < books.length; i++) {
        // Add a dedicated ID getter to Book:
        // if (books[i].getBookId() == id) return i;
    }
    return -1;
}
```

A simple alternative is to add this method to `Book`:

```java
public int getBookId() {
    return bookId;
}
```

Then the search becomes:

```java
public static int findBookIndex(Book[] books, int id) {
    for (int i = 0; i < books.length; i++) {
        if (books[i].getBookId() == id) {
            return i;
        }
    }
    return -1;
}
```

This follows the assignment's requirement that operations are identified by `bookId`. 

