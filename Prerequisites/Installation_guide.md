# Java IDE Installation Guide for Beginners

> **Audience:** Students with no prior programming experience.

This guide explains every step, including where to click, what to
download, where files are usually stored, how to verify installation,
and common mistakes.

------------------------------------------------------------------------

# Part 1 -- Before You Begin

## What is Java?

Java is a programming language. To write and run Java programs you need:

1.  **JDK (Java Development Kit)** -- contains the Java compiler
    (`javac`) and runtime.
2.  **An IDE (Integrated Development Environment)** -- software used to
    write and run code.

We will install one of these IDEs: - Visual Studio Code - Apache
NetBeans - Eclipse IDE

------------------------------------------------------------------------

# Part 2 -- Install the JDK (Required for Every IDE)

## Windows

### Step 1: Download

Visit the official Oracle JDK page or Eclipse Temurin website.

Click the latest **JDK 25 or newer** for **Windows x64 Installer
(.exe)**.

Your browser normally saves it to:

    C:\Users\YOUR_USERNAME\Downloads

### Step 2: Install

1.  Open **File Explorer**.
2.  Click **Downloads**.
3.  Double-click the downloaded `.exe`.
4.  Click **Next** until installation starts.
5.  Leave the default installation folder:

```{=html}
<!-- -->
```
    C:\Program Files\Java\jdk-25

Do not change it unless instructed.

### Step 3: Verify

Open **Command Prompt**.

Type:

``` cmd
java -version
javac -version
```

Both commands should display version information.

### Step 4: Configure PATH (if required)

Open:

Settings → System → About → Advanced System Settings → Environment
Variables.

Create:

    JAVA_HOME

Value:

    C:\Program Files\Java\jdk-25

Edit **Path** and add:

    %JAVA_HOME%\bin

Restart Command Prompt.

------------------------------------------------------------------------

## For Linux users

Install OpenJDK using your package manager.

Ubuntu:

``` bash
sudo apt update
sudo apt install default-jdk
```

Check where Java is installed:

``` bash
which java
readlink -f $(which java)
```

Typical location:

    /usr/lib/jvm/

------------------------------------------------------------------------

## For macOS users

Download a JDK installer or install using Homebrew.

Typical installation location:

    /Library/Java/JavaVirtualMachines/

Verify:

``` bash
java -version
javac -version
```

------------------------------------------------------------------------

# Installing Visual Studio Code

## Download

Go to the official Visual Studio Code website.

Choose the installer for your operating system.

After downloading, the installer is normally inside **Downloads**.

## Install

Run the installer.

Enable these options:

-   Add to PATH
-   Register Code as supported file type
-   Add "Open with Code"

Finish installation.

## Install Java Extension

Open VS Code.

Click the **Extensions** icon.

Search:

    Extension Pack for Java

Click **Install**.

Restart VS Code.

Create a new Java project using the Command Palette.

------------------------------------------------------------------------

# Installing Apache NetBeans

Download the installer from the official Apache NetBeans website.

Windows users should run the installer from Downloads.

Linux users extract the archive and run the executable inside:

    netbeans/bin/

macOS users drag NetBeans into the Applications folder.

When NetBeans starts for the first time it may ask for the JDK location.

Browse to:

Windows:

    C:\Program Files\Java\jdk-25

Linux:

    /usr/lib/jvm/

macOS:

    /Library/Java/JavaVirtualMachines/

Select the JDK folder.

------------------------------------------------------------------------

# Installing Eclipse IDE

Download **Eclipse Installer** from the official Eclipse website.

Run the installer.

Select:

**Eclipse IDE for Java Developers**

Choose the default installation directory.

If Eclipse cannot locate Java automatically, browse to your JDK
installation folder listed above.

------------------------------------------------------------------------


# Java Compilation and Execution Guide


# Example Project

```
Project/
│
├── Main.java

```

Example `Main.java`

```java
public class Main {
    public static void main(String[] args) {
        System.out.println("Hello World!");
    }
}
```

---

# Method 1: Command Line (Windows, Linux, macOS)

## Step 1: Open Terminal

### Windows

- Command Prompt (cmd)
- PowerShell
- Windows Terminal

### Linux

- Terminal

### macOS

- Terminal

---

## Step 2: Navigate to Project Folder

```bash
cd path/to/project
```

Example

Windows

```bash
cd C:\Users\John\Desktop\Project
```

Linux/macOS

```bash
cd ~/Desktop/Project
```

---

## Step 3: Compile

Compile one file:

```bash
javac Main.java
```

Compile multiple files:

```bash
javac *.java
```

If using packages:

```bash
javac -d out src/*.java
```

---

## Step 4: Run

Without packages:

```bash
java Main
```

With packages:

```bash
java -cp out package_name.Main
```

---

# Method 2: Visual Studio Code

## Step 1

Install:

- Visual Studio Code
- Extension Pack for Java

The extension pack includes:

- Language Support for Java
- Debugger for Java
- Maven Support
- Test Runner

---

## Step 2

Open the project folder:

```
File
    Open Folder
```

---

## Step 3

Open `Main.java`.

VS Code automatically detects the Java project.

---

## Step 4

Click either:

- Run
- Run Java

or press

```
Ctrl + F5
```

To debug:

```
F5
```

---

## Compile from VS Code Terminal

Open:

```
Terminal
New Terminal
```

Compile:

```bash
javac Main.java
```

Run:

```bash
java Main
```

---

# Method 3: Eclipse IDE

## Step 1

Open Eclipse.

---

## Step 2

Create a project.

```
File
    New
        Java Project
```

---

## Step 3

Enter a project name.

Click

```
Finish
```

---

## Step 4

Create a class.

```
Right Click src
    New
        Class
```

Check

```
public static void main(String[] args)
```

Click

```
Finish
```

---

## Step 5

Write your Java code.

---

## Step 6

Run the program.

Click

```
Run
```

or

```
Right Click Main.java
    Run As
        Java Application
```

Shortcut:

```
Ctrl + F11
```

---

# Compiling Multiple Files

Suppose you have:

```
Main.java
Student.java
Teacher.java
```

Compile:

```bash
javac *.java
```

Run:

```bash
java Main
```

---

# Compiling Package-Based Projects

Project structure:

```
src/
└── com/
    └── example/
        ├── Main.java
        └── Student.java
```

Compile:

```bash
javac -d out src/com/example/*.java
```

Run:

```bash
java -cp out com.example.Main
```

---

# Common Errors

## Error: javac is not recognized

**Cause**

JDK is not installed or PATH is not configured.

**Solution**

- Install the JDK.
- Add the JDK `bin` directory to the system PATH.
- Restart the terminal.

---

## Error: Could not find or load main class

**Cause**

- Wrong class name
- Wrong package
- Incorrect classpath

**Solution**

- Ensure the class name matches the filename.
- Verify package declarations.
- Use the correct classpath with `-cp`.

Example:

```bash
java -cp out com.example.Main
```

---

## Error: ClassNotFoundException

**Cause**

The JVM cannot locate the compiled class.

**Solution**

Compile first:

```bash
javac Main.java
```

Then run:

```bash
java Main
```

---

# Useful Commands

Compile:

```bash
javac Main.java
```

Compile all Java files:

```bash
javac *.java
```

Run:

```bash
java Main
```

Compile to an output directory:

```bash
javac -d out src/*.java
```

Run with classpath:

```bash
java -cp out Main
```

Run packaged application:

```bash
java -cp out com.example.Main
```

---

# Keyboard Shortcuts

## VS Code

| Action | Shortcut |
|---------|----------|
| Run | Ctrl + F5 |
| Debug | F5 |
| Terminal | Ctrl + ` |

---

## Eclipse

| Action | Shortcut |
|---------|----------|
| Run | Ctrl + F11 |
| Debug | F11 |
| Save | Ctrl + S |

---








# Common Beginner Mistakes

-   Installing a JRE instead of a JDK.
-   Forgetting to install Java extensions in VS Code.
-   Choosing the wrong folder instead of the JDK folder.
-   Not restarting the terminal after changing PATH.
-   Downloading from unofficial websites.

------------------------------------------------------------------------

# Tips

Always download software from official websites.

Leave installation locations as the defaults unless your instructor says
otherwise.

Keep your projects in:

Windows:

    Documents\JavaProjects

Linux:

    ~/JavaProjects

macOS:

    ~/Documents/JavaProjects
