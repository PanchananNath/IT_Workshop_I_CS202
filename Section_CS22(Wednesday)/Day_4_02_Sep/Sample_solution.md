

#  Pseudocode

## Part A

```
CLASS Librarian
    FIELDS: name, librarianId
    CONSTRUCTOR Librarian():
        PROMPT and READ name
        PROMPT and READ librarianId
    METHOD toString(): RETURN formatted "ID | Name" string
    getters / setters for name, librarianId

CLASS Book
    FIELDS: title, bookCode, price, librarian, category
    STATIC FIELD: libraryCode = 1001   // shared by every Book

    CONSTRUCTOR Book():
        // default / empty

    CONSTRUCTOR Book(Librarian assignedLibrarian):
        SET librarian = assignedLibrarian
        PROMPT and READ title, bookCode, price, category

    METHOD updateBookRecord():
        PROMPT and READ new price
        PROMPT and READ new category
        // implemented here; called from the menu starting in Part B

    METHOD getBookDetails():
        PRINT libraryCode, bookCode, title, price, category, librarian.name

    METHOD getLibrarian(): RETURN librarian

MAIN PROGRAM
    librarianList = new ArrayList<Librarian>()
    bookList      = new ArrayList<Book>()
    MAX_LIBRARIANS = 2
    MAX_BOOKS = 5

    LOOP until user selects Exit:
        SHOW menu: 1 Add Librarian | 2 Add Book | 3 Display Books | 4 Display Librarian of a Book | 5 Exit
        READ choice

        IF choice == 1:                       // Add Librarian
            IF librarianList.size() >= MAX_LIBRARIANS:
                PRINT "limit reached" error
            ELSE:
                CREATE new Librarian()          // reads its own input
                librarianList.add(it)

        IF choice == 2:                       // Add Book
            IF bookList.size() >= MAX_BOOKS:
                PRINT "limit reached" error
            ELSE IF librarianList is empty:
                PRINT "add a librarian first" error
            ELSE:
                SHOW numbered list of librarianList
                READ librarian index
                IF index invalid: PRINT error, STOP this operation
                CREATE new Book(librarianList[index])   // reads its own input
                bookList.add(it)

        IF choice == 3:                       // Display Book details
            IF bookList empty: PRINT "no books"
            ELSE: FOR EACH book IN bookList: book.getBookDetails()

        IF choice == 4:                       // Display Librarian of a particular Book
            IF bookList empty: PRINT "no books"
            ELSE:
                SHOW numbered list of bookList (title + code)
                READ book index
                IF index invalid: PRINT error, STOP this operation
                PRINT bookList[index].getLibrarian()

        IF choice == 5: PRINT "Exiting" and END LOOP
```

## Part B

`Librarian` and `Book` classes are unchanged from Part A. Only the menu and driver logic grow:

```
MAIN PROGRAM
    librarianList, bookList, MAX_LIBRARIANS, MAX_BOOKS  // same as Part A

    LOOP until user selects Exit:
        SHOW menu:
            1 Add Book | 2 Delete Book | 3 Add Librarian | 4 Delete Librarian
            5 Update Book | 6 Update Librarian
            7 Display Book details | 8 Display Librarian of a Book | 9 Exit
        READ choice

        IF choice == 1: ... same "Add Book" logic as Part A ...
        IF choice == 3: ... same "Add Librarian" logic as Part A ...
        IF choice == 7: ... same "Display Book details" logic as Part A ...
        IF choice == 8: ... same "Display Librarian of a Book" logic as Part A ...

        IF choice == 2:                       // Delete Book
            IF bookList empty: PRINT "no books"
            ELSE:
                SHOW numbered list of bookList
                READ book index
                IF index invalid: PRINT error, STOP this operation
                bookList.remove(index)
                PRINT "deleted"

        IF choice == 4:                       // Delete Librarian
            IF librarianList empty: PRINT "no librarians"
            ELSE:
                SHOW numbered list of librarianList
                READ librarian index
                IF index invalid: PRINT error, STOP this operation
                librarianList.remove(index)
                PRINT "deleted"

        IF choice == 5:                       // Update Book
            IF bookList empty: PRINT "no books"
            ELSE:
                SHOW numbered list of bookList
                READ book index
                IF index invalid: PRINT error, STOP this operation
                bookList[index].updateBookRecord()   // prompts for new price/category

        IF choice == 6:                       // Update Librarian
            IF librarianList empty: PRINT "no librarians"
            ELSE:
                SHOW numbered list of librarianList
                READ librarian index
                IF index invalid: PRINT error, STOP this operation
                READ new name
                librarianList[index].setName(newName)

        IF choice == 9: PRINT "Exiting" and END LOOP
```

---

# Java Source Code

Both programs were compiled with `javac` (OpenJDK 21) and exercised with sample console input covering: adding librarians up to and past the limit, adding books with librarian assignment, displaying book/librarian details, updating a book and a librarian, and deleting a book and a librarian — all without errors.

Each part is a **separate, self-contained, compilable program**. Keep them in separate folders (e.g. `PartA/` and `PartB/`) since both files define classes named `Librarian` and `Book` — compiling them into the *same* folder would make the second compilation silently overwrite the first's `.class` files.

## 3.1 Part A — `LibraryPartA.java`

*(Menu: Add Librarian · Add Book · Display Book details · Display Librarian details of a book)*

```java
import java.util.ArrayList;
import java.util.Scanner;


class Librarian {
    private String name;
    private String librarianId;

    // No-argument constructor
    public Librarian() {
        System.out.print("Enter Librarian Name: ");
        this.name = LibraryPartA.sc.nextLine();
        System.out.print("Enter Librarian ID  : ");
        this.librarianId = LibraryPartA.sc.nextLine();
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getLibrarianId() {
        return librarianId;
    }

    public void setLibrarianId(String librarianId) {
        this.librarianId = librarianId;
    }

    @Override
    public String toString() {
        return "Librarian ID: " + librarianId + " | Name: " + name;
    }
}



class Book {
    private String title;
    private int bookCode;
    private double price;
    private Librarian librarian;
    private String category; // Fiction, NonFiction, Reference, Periodical

    // static field: one library code shared by every Book object
    private static int libraryCode = 1001;

    // Default constructor
    public Book() {
    }

    // Constructor that also assigns the librarian responsible for this book
    public Book(Librarian librarian) {
        this.librarian = librarian;
        System.out.print("Enter Book Title    : ");
        this.title = LibraryPartA.sc.nextLine();
        System.out.print("Enter Book Code     : ");
        this.bookCode = Integer.parseInt(LibraryPartA.sc.nextLine());
        System.out.print("Enter Book Price    : ");
        this.price = Double.parseDouble(LibraryPartA.sc.nextLine());
        System.out.print("Enter Category (Fiction/NonFiction/Reference/Periodical): ");
        this.category = LibraryPartA.sc.nextLine();
    }

    // Updates price/category. Implemented here; wired into the menu in Part B.
    public void updateBookRecord() {
        System.out.print("Enter new Price for '" + title + "'   : ");
        this.price = Double.parseDouble(LibraryPartA.sc.nextLine());
        System.out.print("Enter new Category for '" + title + "': ");
        this.category = LibraryPartA.sc.nextLine();
        System.out.println("Book record updated successfully.");
    }

    public void getBookDetails() {
        System.out.println("-------------------------------------------");
        System.out.println("Library Code : " + libraryCode);
        System.out.println("Book Code    : " + bookCode);
        System.out.println("Title        : " + title);
        System.out.println("Price        : " + price);
        System.out.println("Category     : " + category);
        System.out.println("Librarian    : " + (librarian != null ? librarian.getName() : "Not Assigned"));
        System.out.println("-------------------------------------------");
    }

    public Librarian getLibrarian() {
        return librarian;
    }

    public int getBookCode() {
        return bookCode;
    }

    public String getTitle() {
        return title;
    }
}


public class LibraryPartA {

    static ArrayList<Librarian> librarianList = new ArrayList<>();
    static ArrayList<Book> bookList = new ArrayList<>();

    static final int MAX_LIBRARIANS = 2;
    static final int MAX_BOOKS = 5;

    // single shared Scanner used by every class in this file
    static Scanner sc = new Scanner(System.in);

    public static void main(String[] args) {
        int choice;
        do {
            System.out.println("\n===== LIBRARY MANAGEMENT SYSTEM (Part A) =====");
            System.out.println("1. Add Librarian");
            System.out.println("2. Add Book");
            System.out.println("3. Display Book Details");
            System.out.println("4. Display Librarian Details (of a particular book)");
            System.out.println("5. Exit");
            System.out.print("Enter your choice: ");
            choice = Integer.parseInt(sc.nextLine().trim());

            switch (choice) {
                case 1:
                    addLibrarian();
                    break;
                case 2:
                    addBook();
                    break;
                case 3:
                    displayBookDetails();
                    break;
                case 4:
                    displayLibrarianOfBook();
                    break;
                case 5:
                    System.out.println("Exiting...");
                    break;
                default:
                    System.out.println("Invalid choice!");
            }
        } while (choice != 5);

        sc.close();
    }

    static void addLibrarian() {
        if (librarianList.size() >= MAX_LIBRARIANS) {
            System.out.println("Error: Maximum limit of " + MAX_LIBRARIANS + " librarians reached!");
            return;
        }
        Librarian l = new Librarian();
        librarianList.add(l);
        System.out.println("Librarian added successfully.");
    }

    static void addBook() {
        if (bookList.size() >= MAX_BOOKS) {
            System.out.println("Error: Maximum limit of " + MAX_BOOKS + " books reached!");
            return;
        }
        if (librarianList.isEmpty()) {
            System.out.println("Error: Add at least one Librarian before adding a Book.");
            return;
        }
        System.out.println("Select Librarian to assign this book to:");
        for (int i = 0; i < librarianList.size(); i++) {
            System.out.println((i + 1) + ". " + librarianList.get(i));
        }
        System.out.print("Enter choice: ");
        int idx = Integer.parseInt(sc.nextLine().trim()) - 1;
        if (idx < 0 || idx >= librarianList.size()) {
            System.out.println("Invalid librarian selection. Book not added.");
            return;
        }
        Book b = new Book(librarianList.get(idx));
        bookList.add(b);
        System.out.println("Book added successfully.");
    }

    static void displayBookDetails() {
        if (bookList.isEmpty()) {
            System.out.println("No books available.");
            return;
        }
        for (Book b : bookList) {
            b.getBookDetails();
        }
    }

    static void displayLibrarianOfBook() {
        if (bookList.isEmpty()) {
            System.out.println("No books available.");
            return;
        }
        System.out.println("Select a book:");
        for (int i = 0; i < bookList.size(); i++) {
            System.out.println((i + 1) + ". " + bookList.get(i).getTitle()
                    + " (Code: " + bookList.get(i).getBookCode() + ")");
        }
        System.out.print("Enter choice: ");
        int idx = Integer.parseInt(sc.nextLine().trim()) - 1;
        if (idx < 0 || idx >= bookList.size()) {
            System.out.println("Invalid book selection.");
            return;
        }
        Librarian l = bookList.get(idx).getLibrarian();
        if (l != null) {
            System.out.println(l);
        } else {
            System.out.println("No librarian assigned to this book.");
        }
    }
}
```

## Part B — `LibraryPartB.java`

*(Full menu: Add Book · Delete Book · Add Librarian · Delete Librarian · Update Book · Update Librarian · Display Book details · Display Librarian details of a book)*

```java
import java.util.ArrayList;
import java.util.Scanner;


class Librarian {
    private String name;
    private String librarianId;

    public Librarian() {
        System.out.print("Enter Librarian Name: ");
        this.name = LibraryPartB.sc.nextLine();
        System.out.print("Enter Librarian ID  : ");
        this.librarianId = LibraryPartB.sc.nextLine();
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getLibrarianId() {
        return librarianId;
    }

    public void setLibrarianId(String librarianId) {
        this.librarianId = librarianId;
    }

    @Override
    public String toString() {
        return "Librarian ID: " + librarianId + " | Name: " + name;
    }
}


class Book {
    private String title;
    private int bookCode;
    private double price;
    private Librarian librarian;
    private String category; // Fiction, NonFiction, Reference, Periodical

    private static int libraryCode = 1001; // common for all books

    public Book() {
    }

    public Book(Librarian librarian) {
        this.librarian = librarian;
        System.out.print("Enter Book Title    : ");
        this.title = LibraryPartB.sc.nextLine();
        System.out.print("Enter Book Code     : ");
        this.bookCode = Integer.parseInt(LibraryPartB.sc.nextLine());
        System.out.print("Enter Book Price    : ");
        this.price = Double.parseDouble(LibraryPartB.sc.nextLine());
        System.out.print("Enter Category (Fiction/NonFiction/Reference/Periodical): ");
        this.category = LibraryPartB.sc.nextLine();
    }

    // Now wired into the "Update Book" menu option
    public void updateBookRecord() {
        System.out.print("Enter new Price for '" + title + "'   : ");
        this.price = Double.parseDouble(LibraryPartB.sc.nextLine());
        System.out.print("Enter new Category for '" + title + "': ");
        this.category = LibraryPartB.sc.nextLine();
        System.out.println("Book record updated successfully.");
    }

    public void getBookDetails() {
        System.out.println("-------------------------------------------");
        System.out.println("Library Code : " + libraryCode);
        System.out.println("Book Code    : " + bookCode);
        System.out.println("Title        : " + title);
        System.out.println("Price        : " + price);
        System.out.println("Category     : " + category);
        System.out.println("Librarian    : " + (librarian != null ? librarian.getName() : "Not Assigned"));
        System.out.println("-------------------------------------------");
    }

    public Librarian getLibrarian() {
        return librarian;
    }

    public int getBookCode() {
        return bookCode;
    }

    public String getTitle() {
        return title;
    }
}


public class LibraryPartB {

    static ArrayList<Librarian> librarianList = new ArrayList<>();
    static ArrayList<Book> bookList = new ArrayList<>();

    static final int MAX_LIBRARIANS = 2;
    static final int MAX_BOOKS = 5;

    static Scanner sc = new Scanner(System.in);

    public static void main(String[] args) {
        int choice;
        do {
            System.out.println("\n===== LIBRARY MANAGEMENT SYSTEM (Part B) =====");
            System.out.println("1. Add Book");
            System.out.println("2. Delete Book");
            System.out.println("3. Add Librarian");
            System.out.println("4. Delete Librarian");
            System.out.println("5. Update Book");
            System.out.println("6. Update Librarian");
            System.out.println("7. Display Book Details");
            System.out.println("8. Display Librarian Details (of a particular book)");
            System.out.println("9. Exit");
            System.out.print("Enter your choice: ");
            choice = Integer.parseInt(sc.nextLine().trim());

            switch (choice) {
                case 1:
                    addBook();
                    break;
                case 2:
                    deleteBook();
                    break;
                case 3:
                    addLibrarian();
                    break;
                case 4:
                    deleteLibrarian();
                    break;
                case 5:
                    updateBook();
                    break;
                case 6:
                    updateLibrarian();
                    break;
                case 7:
                    displayBookDetails();
                    break;
                case 8:
                    displayLibrarianOfBook();
                    break;
                case 9:
                    System.out.println("Exiting...");
                    break;
                default:
                    System.out.println("Invalid choice!");
            }
        } while (choice != 9);

        sc.close();
    }

  

    static void addLibrarian() {
        if (librarianList.size() >= MAX_LIBRARIANS) {
            System.out.println("Error: Maximum limit of " + MAX_LIBRARIANS + " librarians reached!");
            return;
        }
        Librarian l = new Librarian();
        librarianList.add(l);
        System.out.println("Librarian added successfully.");
    }

    static void addBook() {
        if (bookList.size() >= MAX_BOOKS) {
            System.out.println("Error: Maximum limit of " + MAX_BOOKS + " books reached!");
            return;
        }
        if (librarianList.isEmpty()) {
            System.out.println("Error: Add at least one Librarian before adding a Book.");
            return;
        }
        System.out.println("Select Librarian to assign this book to:");
        for (int i = 0; i < librarianList.size(); i++) {
            System.out.println((i + 1) + ". " + librarianList.get(i));
        }
        System.out.print("Enter choice: ");
        int idx = Integer.parseInt(sc.nextLine().trim()) - 1;
        if (idx < 0 || idx >= librarianList.size()) {
            System.out.println("Invalid librarian selection. Book not added.");
            return;
        }
        Book b = new Book(librarianList.get(idx));
        bookList.add(b);
        System.out.println("Book added successfully.");
    }



    static void deleteLibrarian() {
        if (librarianList.isEmpty()) {
            System.out.println("No librarians to delete.");
            return;
        }
        System.out.println("Select Librarian to delete:");
        for (int i = 0; i < librarianList.size(); i++) {
            System.out.println((i + 1) + ". " + librarianList.get(i));
        }
        System.out.print("Enter choice: ");
        int idx = Integer.parseInt(sc.nextLine().trim()) - 1;
        if (idx < 0 || idx >= librarianList.size()) {
            System.out.println("Invalid selection.");
            return;
        }
        librarianList.remove(idx);
        System.out.println("Librarian deleted successfully.");
    }

    static void deleteBook() {
        if (bookList.isEmpty()) {
            System.out.println("No books to delete.");
            return;
        }
        System.out.println("Select Book to delete:");
        for (int i = 0; i < bookList.size(); i++) {
            System.out.println((i + 1) + ". " + bookList.get(i).getTitle()
                    + " (Code: " + bookList.get(i).getBookCode() + ")");
        }
        System.out.print("Enter choice: ");
        int idx = Integer.parseInt(sc.nextLine().trim()) - 1;
        if (idx < 0 || idx >= bookList.size()) {
            System.out.println("Invalid selection.");
            return;
        }
        bookList.remove(idx);
        System.out.println("Book deleted successfully.");
    }



    static void updateBook() {
        if (bookList.isEmpty()) {
            System.out.println("No books available to update.");
            return;
        }
        System.out.println("Select Book to update:");
        for (int i = 0; i < bookList.size(); i++) {
            System.out.println((i + 1) + ". " + bookList.get(i).getTitle()
                    + " (Code: " + bookList.get(i).getBookCode() + ")");
        }
        System.out.print("Enter choice: ");
        int idx = Integer.parseInt(sc.nextLine().trim()) - 1;
        if (idx < 0 || idx >= bookList.size()) {
            System.out.println("Invalid selection.");
            return;
        }
        bookList.get(idx).updateBookRecord();
    }

    static void updateLibrarian() {
        if (librarianList.isEmpty()) {
            System.out.println("No librarians available to update.");
            return;
        }
        System.out.println("Select Librarian to update:");
        for (int i = 0; i < librarianList.size(); i++) {
            System.out.println((i + 1) + ". " + librarianList.get(i));
        }
        System.out.print("Enter choice: ");
        int idx = Integer.parseInt(sc.nextLine().trim()) - 1;
        if (idx < 0 || idx >= librarianList.size()) {
            System.out.println("Invalid selection.");
            return;
        }
        System.out.print("Enter new name: ");
        String newName = sc.nextLine();
        librarianList.get(idx).setName(newName);
        System.out.println("Librarian updated successfully.");
    }



    static void displayBookDetails() {
        if (bookList.isEmpty()) {
            System.out.println("No books available.");
            return;
        }
        for (Book b : bookList) {
            b.getBookDetails();
        }
    }

    static void displayLibrarianOfBook() {
        if (bookList.isEmpty()) {
            System.out.println("No books available.");
            return;
        }
        System.out.println("Select a book:");
        for (int i = 0; i < bookList.size(); i++) {
            System.out.println((i + 1) + ". " + bookList.get(i).getTitle()
                    + " (Code: " + bookList.get(i).getBookCode() + ")");
        }
        System.out.print("Enter choice: ");
        int idx = Integer.parseInt(sc.nextLine().trim()) - 1;
        if (idx < 0 || idx >= bookList.size()) {
            System.out.println("Invalid book selection.");
            return;
        }
        Librarian l = bookList.get(idx).getLibrarian();
        if (l != null) {
            System.out.println(l);
        } else {
            System.out.println("No librarian assigned to this book.");
        }
    }
}
```

---

# How to compile and run

```
# Part A
cd PartA
javac LibraryPartA.java
java LibraryPartA

# Part B
cd PartB
javac LibraryPartB.java
java LibraryPartB
```

Keep the two parts in **separate folders** — both files declare classes named `Librarian` and `Book`, so compiling them into the same directory would cause the second `javac` run to overwrite the first's class files.

