## Assignment 6 

## Pseudocode

```text
START

Create Student array with maximum size 5
Create Faculty array with maximum size 2

Create Person class
    Store name and age
    Constructor initializes name and age
    getDetails() displays person details

Create Faculty class extending Person
    Store empID
    Constructor initializes faculty data
    Override getDetails()
    toString() returns faculty name + empID

Create Student class extending Person
    Store rollNo, cgpa, branch, collegeID
    Store Faculty facultyAdvisor
    Constructor initializes student data
    Constructor(Faculty) assigns facultyAdvisor
    Override getDetails()
    getAdvisor() returns facultyAdvisor

REPEAT
    Display menu
    Read choice

    IF choice = 1
        Add student
        IF number of students < 5
            Read student details
            Read/select faculty advisor
            Store student
        ELSE
            Display "Student limit reached"
        END IF

    ELSE IF choice = 2
        Delete student

    ELSE IF choice = 3
        Add faculty
        IF number of faculty < 2
            Read faculty details
            Store faculty
        ELSE
            Display "Faculty limit reached"
        END IF

    ELSE IF choice = 4
        Delete faculty

    ELSE IF choice = 5
        Add person
        Read person details
        Store/display person

    ELSE IF choice = 6
        Delete person

    ELSE IF choice = 7
        Display student details

    ELSE IF choice = 8
        Display student's advisor details

    ELSE IF choice = 9
        Display person details

    ELSE
        Display "Invalid choice"
    END IF

UNTIL user chooses to exit

END
```

## Java Code

```java
import java.util.*;

class Person {
    protected String name;
    protected int age;

    Person() {
    }

    Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public void getDetails() {
        System.out.println("Name: " + name);
        System.out.println("Age: " + age);
    }
}

class Faculty extends Person {
    private int empID;

    Faculty() {
    }

    Faculty(String name, int age, int empID) {
        super(name, age);
        this.empID = empID;
    }

    @Override
    public void getDetails() {
        System.out.println("Faculty Name: " + name);
        System.out.println("Age: " + age);
        System.out.println("Employee ID: " + empID);
    }

    @Override
    public String toString() {
        return name + " (" + empID + ")";
    }

    public int getEmpID() {
        return empID;
    }
}

class Student extends Person {
    private int rollNo;
    private double cgpa;
    private Faculty facultyAdvisor;
    private String branch;
    private String collegeID;

    Student() {
    }

    Student(String name, int age, int rollNo, double cgpa,
            String branch, String collegeID, Faculty advisor) {
        super(name, age);
        this.rollNo = rollNo;
        this.cgpa = cgpa;
        this.branch = branch;
        this.collegeID = collegeID;
        this.facultyAdvisor = advisor;
    }

    Student(Faculty advisor) {
        this.facultyAdvisor = advisor;
    }

    @Override
    public void getDetails() {
        System.out.println("Student Name: " + name);
        System.out.println("Age: " + age);
        System.out.println("Roll No: " + rollNo);
        System.out.println("CGPA: " + cgpa);
        System.out.println("Branch: " + branch);
        System.out.println("College ID: " + collegeID);
        System.out.println("Faculty Advisor: " + facultyAdvisor);
    }

    public Faculty getAdvisor() {
        return facultyAdvisor;
    }

    public int getRollNo() {
        return rollNo;
    }
}

public class Assignment6 {
    static Scanner sc = new Scanner(System.in);

    static Student[] students = new Student[5];
    static Faculty[] faculties = new Faculty[2];
    static Person[] persons = new Person[10];

    static int studentCount = 0;
    static int facultyCount = 0;
    static int personCount = 0;

    static void addStudent() {
        if (studentCount >= 5) {
            System.out.println("Error: Student limit reached.");
            return;
        }

        System.out.print("Name: ");
        String name = sc.nextLine();

        System.out.print("Age: ");
        int age = Integer.parseInt(sc.nextLine());

        System.out.print("Roll No: ");
        int roll = Integer.parseInt(sc.nextLine());

        System.out.print("CGPA: ");
        double cgpa = Double.parseDouble(sc.nextLine());

        System.out.print("Branch: ");
        String branch = sc.nextLine();

        System.out.print("College ID: ");
        String collegeID = sc.nextLine();

        Faculty advisor = null;
        if (facultyCount > 0) {
            System.out.println("Available Faculty:");
            for (int i = 0; i < facultyCount; i++) {
                System.out.println((i + 1) + ". " + faculties[i]);
            }

            System.out.print("Select advisor number: ");
            int choice = Integer.parseInt(sc.nextLine());

            if (choice >= 1 && choice <= facultyCount) {
                advisor = faculties[choice - 1];
            }
        }

        students[studentCount++] =
            new Student(name, age, roll, cgpa, branch, collegeID, advisor);

        System.out.println("Student added successfully.");
    }

    static void deleteStudent() {
        if (studentCount == 0) {
            System.out.println("No students available.");
            return;
        }

        System.out.print("Enter Roll No to delete: ");
        int roll = Integer.parseInt(sc.nextLine());

        for (int i = 0; i < studentCount; i++) {
            if (students[i].getRollNo() == roll) {
                for (int j = i; j < studentCount - 1; j++) {
                    students[j] = students[j + 1];
                }
                students[--studentCount] = null;
                System.out.println("Student deleted.");
                return;
            }
        }

        System.out.println("Student not found.");
    }

    static void addFaculty() {
        if (facultyCount >= 2) {
            System.out.println("Error: Faculty limit reached.");
            return;
        }

        System.out.print("Faculty Name: ");
        String name = sc.nextLine();

        System.out.print("Age: ");
        int age = Integer.parseInt(sc.nextLine());

        System.out.print("Employee ID: ");
        int empID = Integer.parseInt(sc.nextLine());

        faculties[facultyCount++] = new Faculty(name, age, empID);
        System.out.println("Faculty added successfully.");
    }

    static void deleteFaculty() {
        if (facultyCount == 0) {
            System.out.println("No faculty available.");
            return;
        }

        System.out.print("Enter Employee ID to delete: ");
        int id = Integer.parseInt(sc.nextLine());

        for (int i = 0; i < facultyCount; i++) {
            if (faculties[i].getEmpID() == id) {
                for (int j = i; j < facultyCount - 1; j++) {
                    faculties[j] = faculties[j + 1];
                }
                faculties[--facultyCount] = null;
                System.out.println("Faculty deleted.");
                return;
            }
        }

        System.out.println("Faculty not found.");
    }

    static void addPerson() {
        if (personCount >= persons.length) {
            System.out.println("Person storage is full.");
            return;
        }

        System.out.print("Person Name: ");
        String name = sc.nextLine();

        System.out.print("Age: ");
        int age = Integer.parseInt(sc.nextLine());

        persons[personCount++] = new Person(name, age);
        System.out.println("Person added successfully.");
    }

    static void deletePerson() {
        if (personCount == 0) {
            System.out.println("No persons available.");
            return;
        }

        System.out.print("Enter person name to delete: ");
        String name = sc.nextLine();

        for (int i = 0; i < personCount; i++) {
            if (persons[i].name.equalsIgnoreCase(name)) {
                for (int j = i; j < personCount - 1; j++) {
                    persons[j] = persons[j + 1];
                }
                persons[--personCount] = null;
                System.out.println("Person deleted.");
                return;
            }
        }

        System.out.println("Person not found.");
    }

    static void getStudentDetails() {
        if (studentCount == 0) {
            System.out.println("No students available.");
            return;
        }

        for (int i = 0; i < studentCount; i++) {
            students[i].getDetails();
            System.out.println("--------------------");
        }
    }

    static void getAdvisorDetails() {
        if (studentCount == 0) {
            System.out.println("No students available.");
            return;
        }

        System.out.print("Enter Roll No: ");
        int roll = Integer.parseInt(sc.nextLine());

        for (int i = 0; i < studentCount; i++) {
            if (students[i].getRollNo() == roll) {
                Faculty advisor = students[i].getAdvisor();

                if (advisor != null)
                    advisor.getDetails();
                else
                    System.out.println("No advisor assigned.");

                return;
            }
        }

        System.out.println("Student not found.");
    }

    static void getPersonDetails() {
        if (personCount == 0) {
            System.out.println("No persons available.");
            return;
        }

        for (int i = 0; i < personCount; i++) {
            persons[i].getDetails();
            System.out.println("--------------------");
        }
    }

    public static void main(String[] args) {
        while (true) {
            System.out.println("\n===== MENU =====");
            System.out.println("1. Add Student");
            System.out.println("2. Delete Student");
            System.out.println("3. Add Faculty");
            System.out.println("4. Delete Faculty");
            System.out.println("5. Add Person");
            System.out.println("6. Delete Person");
            System.out.println("7. Get Student Details");
            System.out.println("8. Get Advisor Details");
            System.out.println("9. Get Person Details");
            System.out.println("0. Exit");
            System.out.print("Enter choice: ");

            int choice = Integer.parseInt(sc.nextLine());

            switch (choice) {
                case 1 -> addStudent();
                case 2 -> deleteStudent();
                case 3 -> addFaculty();
                case 4 -> deleteFaculty();
                case 5 -> addPerson();
                case 6 -> deletePerson();
                case 7 -> getStudentDetails();
                case 8 -> getAdvisorDetails();
                case 9 -> getPersonDetails();
                case 0 -> {
                    System.out.println("Program ended.");
                    return;
                }
                default -> System.out.println("Invalid choice.");
            }
        }
    }
}
```

