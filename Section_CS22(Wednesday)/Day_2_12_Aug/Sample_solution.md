# Assignment 2 — Java Programs

This document contains the logic, pseudocode, and Java code for all questions in Assignment 2.

The assignment covers:
1. Array operations using a menu
2. Nearest greater element and circular shift
3. Matrix addition, subtraction, and multiplication
4. Square matrix operations, transpose, and determinants

---

# Question 1 — Array Operations Using Menu

The program takes an integer array from the user and allows:

- Find the sum of all elements
- Find the maximum element
- Search for a given element

## 1A. Find Sum

### Simple Logic

Suppose the array is:

```text
[10, 20, 30, 40]
```

Start with:

```text
sum = 0
```

Then add every element:

```text
0 + 10 = 10
10 + 20 = 30
30 + 30 = 60
60 + 40 = 100
```

Therefore, the sum is `100`.

### Pseudocode

```text
START

Read array

sum = 0

FOR each element in array
    sum = sum + element
END FOR

Display sum

END
```

### Java Code

```java
int sum = 0;

for (int i = 0; i < arr.length; i++) {
    sum = sum + arr[i];
}

System.out.println("Sum = " + sum);
```

---

## 1B. Find Maximum Element

### Simple Logic

Suppose:

```text
[10, 50, 20, 80, 30]
```

Initially assume the first element is maximum:

```text
max = 10
```

Compare each remaining element:

```text
50 > 10  → max = 50
20 > 50  → no
80 > 50  → max = 80
30 > 80  → no
```

Answer:

```text
80
```

### Pseudocode

```text
START

Read array

max = first element

FOR each remaining element
    IF element > max
        max = element
    END IF
END FOR

Display max

END
```

### Java Code

```java
int max = arr[0];

for (int i = 1; i < arr.length; i++) {
    if (arr[i] > max) {
        max = arr[i];
    }
}

System.out.println("Maximum = " + max);
```

---

## 1C. Search for an Element

### Simple Logic

Suppose:

```text
[10, 20, 30, 40, 50]
```

We want to search for `30`.

Check each element:

```text
10 == 30 → No
20 == 30 → No
30 == 30 → Yes
```

So the element is found.

### Pseudocode

```text
START

Read array
Read searchElement

found = false

FOR each element in array
    IF element == searchElement
        found = true
        BREAK
    END IF
END FOR

IF found == true
    Display "Element found"
ELSE
    Display "Element not found"
END IF

END
```

### Java Code

```java
System.out.print("Enter element to search: ");
int search = sc.nextInt();

boolean found = false;

for (int i = 0; i < arr.length; i++) {
    if (arr[i] == search) {
        found = true;
        break;
    }
}

if (found) {
    System.out.println("Element found");
} else {
    System.out.println("Element not found");
}
```

---

## Complete Java Program — Question 1



---

# Question 2 — Nearest Greater Element + Circular Shift

The program should:

1. Find the nearest greater element on the right for every element.
2. Perform a circular shift by `k` elements.

The assignment requires the solution to use only arrays and have time complexity no more than `O(n)`. It also gives the hint to use an additional array as a stack.

---

# 2.1 Nearest Greater Element on Right

Example:

```text
Array = {4, 5, 2, 25, 7, 8}
```

Expected output:

```text
5 25 25 -1 8 -1
```

## What does "nearest greater element on right" mean?

For every element, look to its right and find the first element that is greater.

For `4`:

```text
4 5 2 25 7 8
  ↑
```

The nearest greater element is `5`.

For `5`:

```text
5 2 25
    ↑
```

The nearest greater element is `25`.

For `25`, there is no greater element on its right, so the answer is `-1`.

---

## Simple Logic

Use another array as a stack.

Process the original array from **right to left**.

For every element:

1. Remove elements from the stack that are smaller than or equal to the current element.
2. If the stack is empty, the answer is `-1`.
3. Otherwise, the top of the stack is the nearest greater element.
4. Push the current element onto the stack.

This gives an `O(n)` solution.

### Pseudocode

```text
START

Read array

Create stack array
top = -1

FOR i from n-1 down to 0

    WHILE top >= 0 AND stack[top] <= arr[i]
        top = top - 1
    END WHILE

    IF top == -1
        answer[i] = -1
    ELSE
        answer[i] = stack[top]
    END IF

    top = top + 1
    stack[top] = arr[i]

END FOR

Display answer

END
```

### Java Code



---

# 2.2 Circular Shift by K Elements

## Simple Logic

Suppose:

```text
Array = 1 2 3 4 5
```

A right circular shift by `2` gives:

```text
4 5 1 2 3
```

The elements that move beyond the end come back to the beginning.

For a right shift, calculate the new index using:

```text
newIndex = (i + k) % n
```

Example:

```text
n = 5
k = 2
i = 3

newIndex = (3 + 2) % 5
         = 0
```

So the element at index `3` moves to index `0`.

### Pseudocode

```text
START

Read array
Read k

k = k % n

Create new array

FOR i = 0 to n-1
    newIndex = (i + k) % n
    newArray[newIndex] = arr[i]
END FOR

Display newArray

END
```

### Java Code



---

## Complete Java Program — Question 2



---

# Question 3 — Matrix Operations

The program takes two 2D arrays and provides a menu for:

- Addition
- Subtraction
- Multiplication

---

# 3A. Matrix Addition

Suppose:

```text
A = 1 2
    3 4

B = 5 6
    7 8
```

Add corresponding positions:

```text
1 + 5 = 6
2 + 6 = 8
3 + 7 = 10
4 + 8 = 12
```

Result:

```text
6  8
10 12
```

## Logic

The result at position `[i][j]` is:

```text
result[i][j] = A[i][j] + B[i][j]
```

### Pseudocode

```text
FOR i = 0 to rows-1
    FOR j = 0 to columns-1
        result[i][j] = A[i][j] + B[i][j]
    END FOR
END FOR
```

### Java Code



---

# 3B. Matrix Subtraction

The logic is:

```text
result[i][j] = A[i][j] - B[i][j]
```

### Pseudocode

```text
FOR i = 0 to rows-1
    FOR j = 0 to columns-1
        result[i][j] = A[i][j] - B[i][j]
    END FOR
END FOR
```

### Java Code

```java
for (int i = 0; i < rows; i++) {
    for (int j = 0; j < cols; j++) {
        result[i][j] = A[i][j] - B[i][j];
    }
}
```

---

# 3C. Matrix Multiplication

Suppose:

```text
A = 1 2
    3 4

B = 5 6
    7 8
```

For the first result element:

```text
C[0][0] = 1×5 + 2×7
        = 19
```

For `C[0][1]`:

```text
C[0][1] = 1×6 + 2×8
        = 22
```

Result:

```text
19 22
43 50
```

## Main Formula

```text
C[i][j] = sum of A[i][k] * B[k][j]
```

Matrix multiplication requires three loops.

### Pseudocode

```text
FOR i = 0 to rowsA-1

    FOR j = 0 to columnsB-1

        result[i][j] = 0

        FOR k = 0 to columnsA-1
            result[i][j] =
                result[i][j] + A[i][k] * B[k][j]
        END FOR

    END FOR

END FOR
```

### Java Code



---

## Complete Java Program — Question 3



---

# Question 4 — Square Matrix Operations

The program takes a square 2D array and provides a menu for:

- Display original matrix
- Find transpose
- Find determinant of a `2 × 2` matrix
- Find determinant of a `3 × 3` matrix

---

# 4A. Display Original Matrix

## Logic

Use two loops:

- Outer loop → rows
- Inner loop → columns

### Pseudocode

```text
FOR i = 0 to n-1
    FOR j = 0 to n-1
        PRINT matrix[i][j]
    END FOR
    PRINT new line
END FOR
```

### Java Code



---

# 4B. Transpose

Transpose means:

> Rows become columns and columns become rows.

Example:

```text
Original:

1 2 3
4 5 6
7 8 9
```

Transpose:

```text
1 4 7
2 5 8
3 6 9
```

## Logic

The important statement is:

```text
transpose[j][i] = matrix[i][j]
```

### Pseudocode

```text
FOR i = 0 to n-1
    FOR j = 0 to n-1
        transpose[j][i] = matrix[i][j]
    END FOR
END FOR
```

### Java Code



---

# 4C. Determinant of 2 × 2 Matrix

Suppose:

```text
| a  b |
| c  d |
```

The formula is:

```text
det = (a × d) - (b × c)
```

Example:

```text
| 1  2 |
| 3  4 |
```

Then:

```text
det = (1 × 4) - (2 × 3)
    = 4 - 6
    = -2
```

### Pseudocode

```text
a = matrix[0][0]
b = matrix[0][1]
c = matrix[1][0]
d = matrix[1][1]

determinant = (a * d) - (b * c)

Display determinant
```

### Java Code

```java
int determinant =
    matrix[0][0] * matrix[1][1]
    - matrix[0][1] * matrix[1][0];

System.out.println("Determinant = " + determinant);
```

---

# 4D. Determinant of 3 × 3 Matrix

Suppose:

```text
| a b c |
| d e f |
| g h i |
```

Formula:

```text
det = a(ei - fh)
    - b(di - fg)
    + c(dh - eg)
```

## Simple Way to Remember

Take the first row:

```text
a    b    c
```

Then:

```text
a × its minor
- b × its minor
+ c × its minor
```

So:

```text
a(ei - fh)
- b(di - fg)
+ c(dh - eg)
```

### Pseudocode

```text
a = matrix[0][0]
b = matrix[0][1]
c = matrix[0][2]

d = matrix[1][0]
e = matrix[1][1]
f = matrix[1][2]

g = matrix[2][0]
h = matrix[2][1]
i = matrix[2][2]

det =
    a * (e*i - f*h)
    - b * (d*i - f*g)
    + c * (d*h - e*g)

Display det
```

### Java Code

```java


---

# Complete Java Program — Question 4

---


## Important Conditions

### Matrix Addition/Subtraction

The dimensions must be the same:

```text
Rows A = Rows B
Columns A = Columns B
```

### Matrix Multiplication

```text
Columns of A = Rows of B
```

### Nearest Greater Element

Remember:

```text
Go from RIGHT to LEFT
Use an array as STACK
Pop while stack[top] <= current element
```

### Circular Right Shift

Remember:

```text
newIndex = (i + k) % n
```

