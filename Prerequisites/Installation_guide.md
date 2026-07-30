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

Click the latest **JDK 17 or newer** for **Windows x64 Installer
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
    C:\Program Files\Java\jdk-17

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

    C:\Program Files\Java\jdk-17

Edit **Path** and add:

    %JAVA_HOME%\bin

Restart Command Prompt.

------------------------------------------------------------------------

## Linux

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

## macOS

Download a JDK installer or install using Homebrew.

Typical installation location:

    /Library/Java/JavaVirtualMachines/

Verify:

``` bash
java -version
javac -version
```

------------------------------------------------------------------------

# Part 3 -- Installing Visual Studio Code

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

# Part 4 -- Installing Apache NetBeans

Download the installer from the official Apache NetBeans website.

Windows users should run the installer from Downloads.

Linux users extract the archive and run the executable inside:

    netbeans/bin/

macOS users drag NetBeans into the Applications folder.

When NetBeans starts for the first time it may ask for the JDK location.

Browse to:

Windows:

    C:\Program Files\Java\jdk-17

Linux:

    /usr/lib/jvm/

macOS:

    /Library/Java/JavaVirtualMachines/

Select the JDK folder.

------------------------------------------------------------------------

# Part 5 -- Installing Eclipse IDE

Download **Eclipse Installer** from the official Eclipse website.

Run the installer.

Select:

**Eclipse IDE for Java Developers**

Choose the default installation directory.

If Eclipse cannot locate Java automatically, browse to your JDK
installation folder listed above.

------------------------------------------------------------------------

# Part 6 -- Creating Your First Program

Create a project named:

    HelloWorld

Create a class:

    HelloWorld.java

Paste:

``` java
public class HelloWorld {
    public static void main(String[] args){
        System.out.println("Hello, World!");
    }
}
```

Click **Run**.

Expected output:

    Hello, World!

------------------------------------------------------------------------

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
