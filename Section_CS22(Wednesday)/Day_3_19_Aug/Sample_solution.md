# Assignment 3: IT Workshop I — Solutions

## Question 1 — Positive/Negative Sum Across Test Cases

### Explanation of the Problem

We need to process n test cases, where each test case is an array of integers whose length can differ from the others. Because all input must be supplied up front (n, then the sizes of every test case, then the elements of every test case), we cannot read and process one test case at a time — we must first store everything and then compute the sums.

This is naturally modeled with a 2D "jagged" array in Java: an array of arrays where each row can have a different length.

The approach is:

- Read n, the number of test cases.
- Read an array `sizes` of length n, holding how many elements are in each test case.
- Create a jagged array `testCases[n][]`, allocating row i with length `sizes[i]`, and read its values.
- For each row, loop through its elements, accumulating a positive-sum and a negative-sum, and also add these into running overall totals.
- Print the per-test-case sums, then the overall sums.

### Java Code — TestCaseSum.java

```java
import java.util.Scanner;

public class TestCaseSum {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        System.out.print("Enter number of test cases (n): ");
        int n = sc.nextInt();

        int[] sizes = new int[n];

        System.out.println("Enter the number of elements in each test case:");
        for (int i = 0; i < n; i++) {
            sizes[i] = sc.nextInt();
        }

        int[][] testCases = new int[n][];

        for (int i = 0; i < n; i++) {
            testCases[i] = new int[sizes[i]];

            System.out.println(
                "Enter " + sizes[i] + " elements for test case " + (i + 1) + ":"
            );

            for (int j = 0; j < sizes[i]; j++) {
                testCases[i][j] = sc.nextInt();
            }
        }

        long overallPositiveSum = 0;
        long overallNegativeSum = 0;

        for (int i = 0; i < n; i++) {
            long positiveSum = 0;
            long negativeSum = 0;

            for (int j = 0; j < sizes[i]; j++) {
                int value = testCases[i][j];

                if (value > 0) {
                    positiveSum += value;
                } else if (value < 0) {
                    negativeSum += value;
                }
            }

            System.out.println(
                "Test Case " + (i + 1) +
                ": Positive Sum = " + positiveSum +
                ", Negative Sum = " + negativeSum
            );

            overallPositiveSum += positiveSum;
            overallNegativeSum += negativeSum;
        }

        System.out.println("Overall Positive Sum = " + overallPositiveSum);
        System.out.println("Overall Negative Sum = " + overallNegativeSum);

        sc.close();
    }
}
```

### Code Walkthrough

- `n = sc.nextInt()` reads how many test cases the user will supply.
- `int[] sizes = new int[n]` holds the length of every test case.
- `int[][] testCases = new int[n][]` declares a jagged array.
- `testCases[i] = new int[sizes[i]]` allocates each row to exactly the required size.
- The main computation loop iterates over each test case and each element within it.
- Positive values are added to `positiveSum`; negative values are added to `negativeSum`.
- A value of exactly `0` is skipped because it is neither positive nor negative.
- The per-test-case sums are then added to the overall totals.
- `long` is used for sums as a safety margin against overflow.

---

## Question 2 — Bank Account Management System

### Explanation of the Problem

We need to model a bank `Account` as a class with the given fields:

- account number
- holder name
- account type
- balance
- branch name
- minimum balance

The class also contains two constructors and the following methods:

- `setDetails()` in two overloaded forms
- `getDetails()`
- `deposit()`
- `withdraw()`

The program creates 5 `Account` objects in an array and provides a menu for:

1. Set Details
2. Get Details
3. Deposit Money
4. Withdraw Money
5. Exit

The program uses a private static counter `totalAccounts` to track accounts that have been initialized. The minimum balance is `500.0`, and withdrawals that would reduce the balance below this amount are rejected.

### Fully Fixed Java Code

For online compilers that expect the runnable class to be named `Main`, the main class is named `Main`.

```java
import java.util.Scanner;

class Account {
    private int accountNo;
    private String accountHolderName;
    private String accountType;
    private double balance;
    private String branchName;
    private double minimumBalance;

    private static int totalAccounts = 0;

    public Account() {
        this.accountNo = 0;
        this.accountHolderName = "N/A";
        this.accountType = "N/A";
        this.balance = 0.0;
        this.branchName = "N/A";
        this.minimumBalance = 500.0;
    }

    public Account(double balance) {
        this();
        this.balance = balance;
    }

    public void setDetails() {
        Scanner sc = new Scanner(System.in);

        System.out.print("Enter Account Number: ");
        this.accountNo = sc.nextInt();
        sc.nextLine();

        System.out.print("Enter Account Holder Name: ");
        this.accountHolderName = sc.nextLine();

        System.out.print("Enter Account Type (Savings/Current): ");
        this.accountType = sc.nextLine();

        System.out.print("Enter Initial Balance: ");
        this.balance = sc.nextDouble();
        sc.nextLine();

        System.out.print("Enter Branch Name: ");
        this.branchName = sc.nextLine();

        this.minimumBalance = 500.0;
        totalAccounts++;
    }

    public void setDetails(
        int no,
        String name,
        String type,
        double balance,
        String branch
    ) {
        this.accountNo = no;
        this.accountHolderName = name;
        this.accountType = type;
        this.balance = balance;
        this.branchName = branch;
        this.minimumBalance = 500.0;
        totalAccounts++;
    }

    public void getDetails(int no) {
        if (this.accountNo == no) {
            System.out.println("----- Account Details -----");
            System.out.println("Account No       : " + accountNo);
            System.out.println("Account Holder   : " + accountHolderName);
            System.out.println("Account Type     : " + accountType);
            System.out.println("Balance          : " + balance);
            System.out.println("Branch Name      : " + branchName);
            System.out.println("Minimum Balance  : " + minimumBalance);
        }
    }

    public void deposit(int no, double amount) {
        if (this.accountNo == no) {
            if (amount <= 0) {
                System.out.println("Invalid deposit amount.");
                return;
            }

            this.balance += amount;

            System.out.println(
                "Amount deposited successfully. New Balance = " + balance
            );
        }
    }

    public void withdraw(int no, double amount) {
        if (this.accountNo == no) {
            if (amount <= 0) {
                System.out.println("Invalid withdrawal amount.");
            } else if ((this.balance - amount) < this.minimumBalance) {
                System.out.println(
                    "Insufficient balance. Minimum balance must be maintained: "
                    + minimumBalance
                );
            } else {
                this.balance -= amount;

                System.out.println(
                    "Amount withdrawn successfully. New Balance = " + balance
                );
            }
        }
    }

    public int getAccountNo() {
        return accountNo;
    }

    public static int getTotalAccounts() {
        return totalAccounts;
    }
}

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        Account[] accountArray = new Account[5];

        accountArray[0] = new Account();
        accountArray[1] = new Account();
        accountArray[2] = new Account();
        accountArray[3] = new Account();
        accountArray[4] = new Account();

        System.out.println("Enter details for 5 accounts:");

        for (int i = 0; i < accountArray.length; i++) {
            System.out.println("--- Account " + (i + 1) + " ---");
            accountArray[i].setDetails();
        }

        int choice;

        do {
            System.out.println("===== BANK MENU =====");
            System.out.println("1. Set Details");
            System.out.println("2. Get Details");
            System.out.println("3. Deposit Money");
            System.out.println("4. Withdraw Money");
            System.out.println("5. Exit");
            System.out.print("Enter your choice: ");

            choice = sc.nextInt();

            int accNo;
            double amount;
            boolean found;

            switch (choice) {
                case 1:
                    System.out.print(
                        "Enter index of account to set (0-4): "
                    );

                    int idx = sc.nextInt();

                    if (idx >= 0 && idx < accountArray.length) {
                        accountArray[idx].setDetails();
                    } else {
                        System.out.println("Invalid index.");
                    }

                    break;

                case 2:
                    System.out.print(
                        "Enter Account Number to view details: "
                    );

                    accNo = sc.nextInt();
                    found = false;

                    for (Account acc : accountArray) {
                        if (acc.getAccountNo() == accNo) {
                            acc.getDetails(accNo);
                            found = true;
                        }
                    }

                    if (!found) {
                        System.out.println("Account not found.");
                    }

                    break;

                case 3:
                    System.out.print(
                        "Enter Account Number to deposit into: "
                    );

                    accNo = sc.nextInt();

                    System.out.print("Enter amount to deposit: ");
                    amount = sc.nextDouble();

                    found = false;

                    for (Account acc : accountArray) {
                        if (acc.getAccountNo() == accNo) {
                            acc.deposit(accNo, amount);
                            found = true;
                        }
                    }

                    if (!found) {
                        System.out.println("Account not found.");
                    }

                    break;

                case 4:
                    System.out.print(
                        "Enter Account Number to withdraw from: "
                    );

                    accNo = sc.nextInt();

                    System.out.print("Enter amount to withdraw: ");
                    amount = sc.nextDouble();

                    found = false;

                    for (Account acc : accountArray) {
                        if (acc.getAccountNo() == accNo) {
                            acc.withdraw(accNo, amount);
                            found = true;
                        }
                    }

                    if (!found) {
                        System.out.println("Account not found.");
                    }

                    break;

                case 5:
                    System.out.println(
                        "Exiting... Total accounts managed: "
                        + Account.getTotalAccounts()
                    );

                    break;

                default:
                    System.out.println("Invalid choice. Try again.");
            }

        } while (choice != 5);

        sc.close();
    }
}
```

### Important Fix

The original code's main class was:

```java
public class BankManagementSystem
```

It has been changed to:

```java
public class Main
```

This fixes the online compiler error:

```text
ERROR!
error: can't find main(String[]) method in class: Account
```

The `main()` method is inside `Main`, so online compilers that automatically execute `Main` can find it.

### Code Walkthrough

- `Account` contains the six private fields required for the bank account.
- `Account()` initializes default values.
- `Account(double balance)` is the overloaded constructor and reuses the default constructor with `this()`.
- `setDetails()` asks the user for account information.
- The overloaded `setDetails(...)` accepts account information directly through parameters.
- `getDetails(int no)` displays details when the account number matches.
- `deposit()` validates the amount and increases the balance.
- `withdraw()` validates the amount and prevents the balance from going below `500.0`.
- `getAccountNo()` allows the main program to locate an account.
- `getTotalAccounts()` returns the static account counter.
- `Main` creates an array containing five `Account` objects.
- The menu repeatedly allows the user to set details, view details, deposit, withdraw, or exit.
- The program ends when the user selects option 5.

---



