## Assignment 4 



# 1. Basic idea

Think of the program like a small bank.

We have many `Account` objects:

```text
Account 1 → accountNo = 101, balance = 5000
Account 2 → accountNo = 102, balance = 8000
Account 3 → accountNo = 103, balance = 3000
```

We store them in an array:

```java
Account[] accounts = new Account[n];
```

The user then gets a menu:

```text
1. Create Account
2. Update Details
3. Get Details
4. Deposit
5. Withdraw
6. Get Balance
7. Total Accounts
8. Compare Two Accounts
9. Exit
```

The account number tells us which account to operate on.

---

# 2. Account class

The important fields are:

```text
accountNumber
accountType
serviceBranchIFSC
minimumBalance
availableBalance
customerID
customerName
totalAccountCreated
```

`totalAccountCreated` is static because the total belongs to the whole `Account` class, not to one individual account.

Whenever a new account is created:

```java
totalAccountCreated++;
```

---

# 3. Finding an account

Most operations first need to locate an account using its account number.

### Pseudocode

```text
findAccount(accountNumber):

    FOR every account in the array

        IF account.accountNumber == accountNumber
            return that account

    return null
```

Because the accounts are stored in an array, we may have to check every account.

Therefore searching takes:

```text
O(n)
```

where `n` is the number of accounts.

---

# 4. Create Account

### Logic

1. Create an `Account` object.
2. Ask the user for its details.
3. Put it into the array.
4. Increase the account counter.

### Pseudocode

```text
Create Account:

    IF array is full
        print "Maximum account limit reached"
    ELSE
        create Account object
        take account details
        store object in array
        increase account count
```

### Complexity

```text
Time: O(1)
```

The number of fields is fixed, so creating one account takes a constant amount of work.

---

# 5. Deposit

### Logic

Suppose:

```text
Current balance = 5000
Deposit = 2000
```

Then:

```text
New balance = 7000
```

### Pseudocode

```text
deposit(accountNumber, amount):

    find the account

    IF account does not exist
        print "Account not found"

    ELSE IF amount <= 0
        print "Invalid amount"

    ELSE
        availableBalance = availableBalance + amount
```

### Complexity

Finding the account takes `O(n)` and adding the amount takes `O(1)`.

Therefore:

```text
O(n) + O(1) = O(n)
```

---

# 6. Withdraw

The withdrawal should not allow the balance to fall below the minimum balance.

Example:

```text
Balance = 5000
Minimum balance = 1000
Withdraw = 3000

Remaining = 2000
```

This is allowed.

But:

```text
Balance = 5000
Minimum balance = 1000
Withdraw = 4500

Remaining = 500
```

This is not allowed because `500 < 1000`.

### Pseudocode

```text
withdraw(accountNumber, amount):

    find the account

    IF account does not exist
        print "Account not found"

    ELSE IF amount <= 0
        print "Invalid amount"

    ELSE IF availableBalance - amount < minimumBalance
        print "Withdrawal denied"

    ELSE
        availableBalance = availableBalance - amount
```

### Complexity

```text
Searching = O(n)
Calculation/check = O(1)

Total = O(n)
```

---

# 7. Get Details

### Pseudocode

```text
getDetails(accountNumber):

    find the account

    IF found
        display all account details
    ELSE
        print "Account not found"
```

### Complexity

We may need to search all `n` accounts.

```text
Time = O(n)
```

---

# 8. Update Details

The assignment specifically asks for a list of options inside `updateDetails(accountNo)`.

Example:

```text
1. Account Type
2. IFSC
3. Minimum Balance
4. Customer ID
5. Customer Name
```

### Pseudocode

```text
updateDetails(accountNumber):

    find account

    IF account does not exist
        print "Account not found"
        return

    display update menu

    take choice

    SWITCH choice

        CASE 1:
            update account type

        CASE 2:
            update IFSC

        CASE 3:
            update minimum balance

        CASE 4:
            update customer ID

        CASE 5:
            update customer name

        DEFAULT:
            print "Invalid choice"
```

### Complexity

Searching takes `O(n)`.

The switch has a fixed number of cases, so it is `O(1)`.

Therefore:

```text
O(n) + O(1) = O(n)
```

---

# 9. Total number of accounts

The total account counter is static:

```java
private static int totalAccountCreated = 0;
```

Every time an account is created:

```java
totalAccountCreated++;
```

To display it:

```java
Account.totalAccount();
```

### Complexity

There is no loop or search.

```text
Time = O(1)
```

---

# 10. Compare two accounts

The assignment says to compare **only the available balances** and display the details of the account with the greater balance.

Suppose:

```text
Account 101 → 10000
Account 102 → 7000
```

Then account 101 is displayed.

### Pseudocode

```text
compare(account1, account2):

    IF account1.availableBalance > account2.availableBalance
        display account1 details

    ELSE IF account2.availableBalance > account1.availableBalance
        display account2 details

    ELSE
        print "Both accounts have equal balance"
```



```java
import java.util.Scanner;

class Account {

    private int accountNumber;
    private String accountType;
    private String serviceBranchIFSC;
    private float minimumBalance;
    private float availableBalance;
    private int customerID;
    private String customerName;

    private static int totalAccountCreated = 0;

    Scanner sc = new Scanner(System.in);

    Account() {
        totalAccountCreated++;
    }

    public void setDetails() {

        System.out.print("Enter Account Number: ");
        accountNumber = sc.nextInt();
        sc.nextLine();

        System.out.print("Enter Account Type: ");
        accountType = sc.nextLine();

        System.out.print("Enter Service Branch IFSC: ");
        serviceBranchIFSC = sc.nextLine();

        System.out.print("Enter Minimum Balance: ");
        minimumBalance = sc.nextFloat();

        System.out.print("Enter Available Balance: ");
        availableBalance = sc.nextFloat();

        System.out.print("Enter Customer ID: ");
        customerID = sc.nextInt();
        sc.nextLine();

        System.out.print("Enter Customer Name: ");
        customerName = sc.nextLine();
    }

    public String getDetails(int accountNo) {

        if (accountNumber == accountNo) {

            return "\nAccount Number: " + accountNumber
                    + "\nAccount Type: " + accountType
                    + "\nIFSC: " + serviceBranchIFSC
                    + "\nMinimum Balance: " + minimumBalance
                    + "\nAvailable Balance: " + availableBalance
                    + "\nCustomer ID: " + customerID
                    + "\nCustomer Name: " + customerName;
        }

        return null;
    }

    public void updateDetails(int accountNo) {

        if (accountNumber != accountNo) {
            return;
        }

        System.out.println("\nWhat do you want to update?");
        System.out.println("1. Account Type");
        System.out.println("2. IFSC");
        System.out.println("3. Minimum Balance");
        System.out.println("4. Customer ID");
        System.out.println("5. Customer Name");

        System.out.print("Enter choice: ");
        int choice = sc.nextInt();
        sc.nextLine();

        switch (choice) {

            case 1:
                System.out.print("Enter new Account Type: ");
                accountType = sc.nextLine();
                break;

            case 2:
                System.out.print("Enter new IFSC: ");
                serviceBranchIFSC = sc.nextLine();
                break;

            case 3:
                System.out.print("Enter new Minimum Balance: ");
                minimumBalance = sc.nextFloat();
                break;

            case 4:
                System.out.print("Enter new Customer ID: ");
                customerID = sc.nextInt();
                break;

            case 5:
                System.out.print("Enter new Customer Name: ");
                customerName = sc.nextLine();
                break;

            default:
                System.out.println("Invalid choice!");
        }

        System.out.println("Details updated successfully.");
    }

    public float getBalance(int accountNo) {

        if (accountNumber == accountNo) {
            return availableBalance;
        }

        return -1;
    }

    public void deposit(int accountNo, float amount) {

        if (accountNumber != accountNo) {
            return;
        }

        if (amount <= 0) {
            System.out.println("Invalid amount.");
            return;
        }

        availableBalance += amount;

        System.out.println("Amount deposited successfully.");
        System.out.println("New Balance: " + availableBalance);
    }

    public void withdraw(int accountNo, float amount) {

        if (accountNumber != accountNo) {
            return;
        }

        if (amount <= 0) {
            System.out.println("Invalid amount.");
            return;
        }

        if (availableBalance - amount < minimumBalance) {
            System.out.println("Withdrawal denied.");
            System.out.println(
                "Minimum balance must be maintained: "
                + minimumBalance
            );
            return;
        }

        availableBalance -= amount;

        System.out.println("Amount withdrawn successfully.");
        System.out.println("New Balance: " + availableBalance);
    }

    public static int totalAccount() {
        return totalAccountCreated;
    }

    public static void compare(Account account1, Account account2) {

        if (account1.availableBalance > account2.availableBalance) {

            System.out.println("\nAccount 1 has higher balance.");
            System.out.println(
                account1.getDetails(account1.accountNumber)
            );

        } else if (account2.availableBalance > account1.availableBalance) {

            System.out.println("\nAccount 2 has higher balance.");
            System.out.println(
                account2.getDetails(account2.accountNumber)
            );

        } else {

            System.out.println("Both accounts have equal balance.");
        }
    }
}


public class BankingApplication {

    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);

        System.out.print("Enter maximum number of accounts: ");
        int n = sc.nextInt();

        Account[] accounts = new Account[n];

        int accountCount = 0;
        int choice;

        do {

            System.out.println("\n========== BANKING SYSTEM ==========");
            System.out.println("1. Create Account");
            System.out.println("2. Update Details");
            System.out.println("3. Get Details");
            System.out.println("4. Deposit");
            System.out.println("5. Withdraw");
            System.out.println("6. Get Balance");
            System.out.println("7. Total Accounts");
            System.out.println("8. Compare Two Accounts");
            System.out.println("9. Exit");

            System.out.print("Enter your choice: ");
            choice = sc.nextInt();

            switch (choice) {

                case 1:

                    if (accountCount == n) {

                        System.out.println(
                            "Maximum account limit reached."
                        );

                    } else {

                        accounts[accountCount] = new Account();
                        accounts[accountCount].setDetails();
                        accountCount++;

                        System.out.println(
                            "Account created successfully."
                        );
                    }

                    break;

                case 2: {

                    System.out.print("Enter Account Number: ");
                    int accNo = sc.nextInt();

                    boolean found = false;

                    for (int i = 0; i < accountCount; i++) {

                        if (accounts[i].getDetails(accNo) != null) {

                            accounts[i].updateDetails(accNo);
                            found = true;
                            break;
                        }
                    }

                    if (!found) {
                        System.out.println("Account not found.");
                    }

                    break;
                }

                case 3: {

                    System.out.print("Enter Account Number: ");
                    int accNo = sc.nextInt();

                    boolean found = false;

                    for (int i = 0; i < accountCount; i++) {

                        String details =
                            accounts[i].getDetails(accNo);

                        if (details != null) {

                            System.out.println(details);
                            found = true;
                            break;
                        }
                    }

                    if (!found) {
                        System.out.println("Account not found.");
                    }

                    break;
                }

                case 4: {

                    System.out.print("Enter Account Number: ");
                    int accNo = sc.nextInt();

                    System.out.print("Enter amount to deposit: ");
                    float amount = sc.nextFloat();

                    boolean found = false;

                    for (int i = 0; i < accountCount; i++) {

                        if (accounts[i].getDetails(accNo) != null) {

                            accounts[i].deposit(accNo, amount);
                            found = true;
                            break;
                        }
                    }

                    if (!found) {
                        System.out.println("Account not found.");
                    }

                    break;
                }

                case 5: {

                    System.out.print("Enter Account Number: ");
                    int accNo = sc.nextInt();

                    System.out.print("Enter amount to withdraw: ");
                    float amount = sc.nextFloat();

                    boolean found = false;

                    for (int i = 0; i < accountCount; i++) {

                        if (accounts[i].getDetails(accNo) != null) {

                            accounts[i].withdraw(accNo, amount);
                            found = true;
                            break;
                        }
                    }

                    if (!found) {
                        System.out.println("Account not found.");
                    }

                    break;
                }

                case 6: {

                    System.out.print("Enter Account Number: ");
                    int accNo = sc.nextInt();

                    boolean found = false;

                    for (int i = 0; i < accountCount; i++) {

                        float balance =
                            accounts[i].getBalance(accNo);

                        if (balance != -1) {

                            System.out.println(
                                "Available Balance: " + balance
                            );

                            found = true;
                            break;
                        }
                    }

                    if (!found) {
                        System.out.println("Account not found.");
                    }

                    break;
                }

                case 7:

                    System.out.println(
                        "Total Accounts Created: "
                        + Account.totalAccount()
                    );

                    break;

                case 8: {

                    System.out.print(
                        "Enter first Account Number: "
                    );
                    int acc1 = sc.nextInt();

                    System.out.print(
                        "Enter second Account Number: "
                    );
                    int acc2 = sc.nextInt();

                    Account first = null;
                    Account second = null;

                    for (int i = 0; i < accountCount; i++) {

                        if (accounts[i].getDetails(acc1) != null) {
                            first = accounts[i];
                        }

                        if (accounts[i].getDetails(acc2) != null) {
                            second = accounts[i];
                        }
                    }

                    if (first == null || second == null) {

                        System.out.println(
                            "One or both accounts not found."
                        );

                    } else {

                        Account.compare(first, second);
                    }

                    break;
                }

                case 9:
                    System.out.println("Thank you!");
                    break;

                default:
                    System.out.println("Invalid choice!");
            }

        } while (choice != 9);

        sc.close();
    }
}
```

