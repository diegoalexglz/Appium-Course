# TROUBLESHOOTING

### Emulator doesn't start

> Error: 'Connecting to the emulator'

1. Clic on File -> Settings -> Tools -> Emulator
2.  Uncheck 'Launch in the Running Devices tool window'

Note: 

FTools -> Emulator -> Uncheck “Launch in a tool window”.



# BASICS

### Definition

Appium is a automation tool for native apps (android, ios) and mobile browsers. It internally uses WebDriver Json wire (like selenium) to test the apps. So, it's like selenium but for mobile.

### Languages supported by WebDriver

- C#
- Java
- Javascript
- Python
- Ruby

### Appium architecture

1. Appium client code: we write the script and it is exported as Json, no matter the language.

   ---- JSON --->

2. Appium server: it opens a port that 'listens' to the script.

   ---- UIAutomator2 --->	(this is the testing framework)

3. Android

or, XCUITest for ¡os.



# INSTALLATION

### Downloads and environmental variables

1. Download Java and add it to Environment System Variables.

   ```bash
   C:\Program Files\Java\jdk-21
   ```

2. Download Android Studio (in this case, version 2023.1.1.28), find out the Android SDK path (when installing) and add it to Environment System Variables.

   ```bash
   C:\Users\Diego\AppData\Local\Android\Sdk
   ```

   Note: There are some extra step needed to see the 'tools folder' in the sdk, as follows:

   - Open AS

   - Tools -> SDK Manager -> SDK Tools

   - Uncheck 'Hide obsolete packages'

   - Check 'Android SDK Tools (obsolete)'

   - Apply

   And we add these to the env variables aswell:

   ```bash
   C:\Users\Diego\AppData\Local\Android\Sdk\tools\bin
   
   C:\Users\Diego\AppData\Local\Android\Sdk\platform-tools  
   ```

3. Download Node and add it to Environment System Variables.

   ```bash
   C:\Program Files\nodejs
   ```

   We'll also need this to be added to env variables:

   ```bash
   C:\Program Files\nodejs\node_modules\npm\bin
   ```

   (NPM is a tool to install nodejs modules)

   

### Android Studio and AVD

1. Open AS

2. New project -> Basic Activity

3. Tools -> Device manager -> Create Virtual Device

4. **Device definition:** Select '**Pixel 7 Pro**'

5. **System image:** Select '**UpsideDownCake**' (android 14.0) from **x86 Images**.

   Note: It's important to select the image from 'x86 Images' tab and not from 'Recommended', since the last one won't allow to change RAM allocation.

6. **Verify config:** write an AVD name, e.g. 'Pixel 7 Pro API 34'. Here, I also set the RAM to **4096** and the emulated performance to '**Hardware**'.

7. Finish

8. Start the device (we'll see the phone)



### Appium Server

Note: Appium server needs to be started before running any test. By default, it is connected to 4723 port.

1. **Install Appium via node:** Run Powershell as administrator and paste:

   ```bash
   npm install -g appium
   ```

2. **Verify installation:** paste the following:

   ```bash
   appium --version
   ```

   In this case, we've installed 2.4.1

   And the home path is: C:\Users\Diego\.appium

3. Start Appium server: paste the following:

   ```bash
   appium
   ```

   Among the logs, we have:

   - Appium version
   - Home path
   - Connection URLs
     - accessible from other devices on the network
     - accessible only from the same computer
   - Drivers
   - Plugins

   And we must see THIS log:

   ```bash
   Appium REST http interface listener started on http://0.0.0.0:4723
   ```

   

### Eclipse Editor

Eclipse Editor will be used to write our Appium code.

1. Download 'Eclipse IDE for Java Developers' from the web.

2. Open Eclipse

   The default workspace is:

   ```bash
   C:\Users\Diego\eclipse-workspaces
   ```

   

### Java Client (from Maven)

Just like Node NPM helps with packages installation, **Maven repositories host Java libraries**, so, it's quite convenient to setup a maven project, making it easier to pull the libraries we need.

Maven also **manages dependencies** (search, download and organize the libraries we need), automate tasks (build, run tests, generate reports) and **facilitates collaboration** by defining a clear and consistent project model, allowing other developers to work on them without problems.

1. Go to https://mvnrepository.com/

2. Click on '**Java client** for Appium Mobile Webdriver'

   Java-client is the library that allows our Java code to communicate with Appium.

3. Select the latest version available (in this case, version 9.1.0)

   Then, we get the info we need to put on our maven project. In this case:

   ```xml
   <!-- https://mvnrepository.com/artifact/io.appium/java-client -->
   <dependency>
       <groupId>io.appium</groupId>
       <artifactId>java-client</artifactId>
       <version>9.1.0</version>
   </dependency>
   ```



### Framework: TestNG

Similarly, we'll search for our testing framework. In this case, we'll be using TestNG and not JUnit.

1. Go to https://mvnrepository.com/

2. Click on 'TestNG'

3. Select the latest version available (in this case, version 7.9.0)

   Then, we get the info we need to put on our maven project. In this case:

   ```xml
   <!-- https://mvnrepository.com/artifact/org.testng/testng -->
   <dependency>
       <groupId>org.testng</groupId>
       <artifactId>testng</artifactId>
       <version>7.9.0</version>
       <scope>test</scope>
   </dependency>
   ```



### New Project: Eclipse + Maven

1. Open Eclipse

2. Click on File -> New -> Project -> Maven Project

3. **Select an archetype:** search for 'archetype-quickstart' in the text-box and select the one with the group id 'org.apache.maven.archetypes' (in our case, version 1.4).

   This is a template with a predefined basic structure for general java testing. It includes:

   - A **pom.xml** file with basic Maven dependencies

   - A **src/main/java** directory for the project source code

   - A **src/test/java** directory for the project's unit tests

   - A **target directory** for the compiled and packaged project files

   Archetype-quickstart also includes the basic configuration for unit testing with JUnit.

   The src/test/java directory is configured so that Maven automatically runs unit tests with the *mvn* test command.

4. Fill the required fields with our own info. In this case:

   **GroupID** (kinda the package name, or the company's name): **alex.appium**

   **ArtifactId** (kinda the project name, or the feature's name): **project0**

   (so, at the end, our package is alex.appium.project0)

   Note something here. This is very similar to the info we saw from the java-client, and this is because java-client is also a maven project, but made by the appium crew (io.appium, java-client).

5. Click on finish (our project will be opened in a dark eclipse window)

6. Open pom.xml

7. Remove the lines of code of JUnit, because we'll be working with TestNG framework.

   Default JUnit lines of code in pom.xml are these:

   ```xml
     <dependencies>
       <dependency>
         <groupId>junit</groupId>
         <artifactId>junit</artifactId>
         <version>4.11</version>
         <scope>test</scope>
       </dependency>
     </dependencies>
   ```

8. Add the lines of code of **Java-Client and TestNG** framework **into the pom.xml** file:

   ```xml
     <dependencies>
       <dependency>
         <groupId>io.appium</groupId>
         <artifactId>java-client</artifactId>
         <version>9.1.0</version>
         <scope>test</scope>
       </dependency>
       <dependency>
         <groupId>org.testng</groupId>
         <artifactId>testng</artifactId>
         <version>7.9.0</version>
         <scope>test</scope>
       </dependency>
     </dependencies>
   ```

   

9. Delete the AppTest.java files that come by default in ../main/java and ../test/java folders, as they are just examples.

10. **Install TestNG Plugin for eclipse**: click on Help -> Eclipse Marketplace -> TestNG for eclipse -> Install

11. **TIP:** Verify that the 'Build automatically' option in Tools is checked, so there's a scan for problems everytime we modify and save a file. | This scan can also be done by right-clicking the name of our project and clicking on Maven -> Update project.

Now, in 'Maven Dependencies' at the left panel, we can see all the jars that both java-client and testng have automatically downloaded for us.



# Core Java basics for automation (in Eclipse)

### Classes

> CREATE A CLASS

1. Right-click on **src** icon in eclipse's left panel -> new -> class

2. Name it. e.g. 'BrushUp1'

3. ✅ 'public static void main(String[] args)' below 'Which method stubs would you like to create'

   This will come up:

   ```java
   package alex.appium.project0;
   
   public class BrushUp1 {
   
   	public static void main(String[] args) {
   		// TODO Auto-generated method stub
   	}
   }
   ```

   The whole execution will take place inside of the main block, which is inside the  `...args){	}` brackets. Any code outside that block won't be executed.



### Variables and Data Types

> DEFINE A VARIABLE

Any variable needs a type when defined. For example:

```java
int myNum = 5;
float myFloat = 5.5;

char myChar = 'A';
String myName = "Alex";

boolean myBool = true;
```



> PRINT A VARIABLE

We use:

```java
System.out.println(myNum);
```

So, the complete example would be:

```java
package alex.appium.project0;

public class BrushUp1 {

	public static void main(String[] args) {
		int myNum = 5;
        System.out.println(myNum);
	}
}
```

We can run it with:

​					Run as Java Application

------------------------------   **ALT SHIFT X, J**   ------------------------------



and see the result in the console, at the bottom.



> PRINT FORMATTED: CONCATENATE INT AND STRING

We use `+` to concatenate int and string values.

```java
package alex.appium.project0;

public class BrushUp1 {

	public static void main(String[] args) {
		int myNum = 5;
        System.out.println(myNum + " is the value stored in myNum");
	}
}
```



### Arrays

> INITIALIZE AN ARRAY

`[]` and `new` are used to initialize an array.

```java
int[] myArray = new int[3];			// array which can store 5 values
```

> ASSIGN VALUES TO AN ARRAY

Values are assigned starting from zero (0) index.

```java
myArray[0] = 10;
myArray[1] = 20;
myArray[2] = 30;
```

> DEFINE AND ASSIGN VALUES FROM THE START TO AN ARRAY

`{}` are used if we're assigning values from the start to an array.

```java
int[] myIntArray = {10, 20, 30};
String[] myStringArray = {"Alex", "Alvarez"};
```



### For loops

> FOR LOOP: ITERATE OVER AN ARRAY

we can use `.length` to iterate over all the elements of an array.

```java
for(int i=0; i<myIntArray.length; i++){
    System.out.println(myIntArray[i]);
}
```

> ENHANCED FOR LOOP: ITERATE OVER AN ARRAY

```java
for(int i: myIntArray){					// similar to python syntax (for i in myVar)
    System.out.println(i);
}
```



### Conditions

> FOR, IF & MODULE: RETRIEVE EVEN NUMBERS

```java
int[] oneToTen = {1,2,3,4,5,6,7,8,9,10};

for(int i: oneToTen){							// i starts in 0 index
    if(i % 2 == 0){
        System.out.println(i);
    }
}
```



### Arrays lists

A problem that comes implicitly with the classic initialize-later-assign arrays is that the array's **memory allocation** has to be set when defined, so the size of the array is fixed since the beginning. 

> DECLARATION

So, here comes the concept of array lists, which are kinda '**dynamic arrays**', and are declared as follows:

```java
ArrayList<String> myArrayList = new ArrayList<>();
```

> IMPORT CLASS

Note that an ArrayList is a Java Class, and any variable we create that belongs to that type, like **myArrayList**, is known as an **object of the class**.

We can notice this because the reserved word 'ArrayList' is underlined like this <u>ArrayList</u>, and if we click on 'Quick fix' we'll be suggested to import **java.util.ArrayList**.

ArrayList is a class, while java.util is a collection of classes.

```java
package alex.appium.project0;

import java.util.ArrayList; 

public class BrushUp1 {

	public static void main(String[] args) {
		ArrayList<String> myArrayList = new ArrayList<>();
	}
}
```



> ASSIGN VALUES

The way to assign values to an array list is using `add`. Notice that add is known as a **method** of the object.

```java
// object.method
myArrayList.add("Alex")
myArrayList.add("Alvarez")
```

There are many methods, like .remove() or .clear() and this is an advantage of Eclipse, because as we are typing, it is giving us suggestions of the existing methods we could use.



> ACCESS AN ELEMENT

We use the `get` method.

```java
myArrayList.get(0)
```



### Strings and methods

> STRINGS AS OBJECTS: STRING LITERALS

Let's clarify something. **A string is algo an object**, which can be declared as:

```java
String myString = "Alex";
```

So, **if there are two variables which are equal** (they both have the same content), like if we had

```java
String myString1 = "Alex";
String myString2 = "Alex";
```

then, in that case, **Java won't create a new object, but** instead it will fast-check if that specific string is present in an existing variable and if it is, **it will point the second variable to the first**. When declaring strings this way, they're known as string literals.



> MAKING SURE EVERY STRING IS A NEW OBJECT

To ensure that every string we create is a new object, regardless of repeated contents, we can use `new` and `()`.

```java
String myString1 = new String("Alex");
String myString2 = new String("Alex");
```



So, we can make this statement:

-- *A String isn't a sequence of characters, a String is an object that represents a sequence of characters.* --



> SPLIT METHOD

It is used along with an Array to split a String based upon a whitespace or any other character, for example:

```java
String myPhrase = new String("Alex is 21");
String[] mySplittedPhrase = myPhrase.split(" ");
```

We can also use a complete word to work as a separator, so the characters to the left become a element and the characters to the right become another.

```java
String[] mySplittedPhrase = myPhrase.split("is");
```



> TRIM METHOD

To remove whitespaces at the start and end of a String, we can use `Trim()` method.

```java
String noSpaces = mySplittedPhrase[0].trim();
```



> RETRIEVE CHARS: FOR LOOP

We use `charAt()` method to retrieve elements one by one in a string.

```java
String myString = new String("Alex is 21");
for(int i=0; i<s.length(); i++){
    System.out.println(myString.charAt(i));
}
```



### Methods

> WHY METHODS?
>

A method is quite useful because it encapsules blocks of code we want to reuse and give it a name.



> DECLARING A METHOD

A method needs to be declared outside of the main block, that is, outside the  `...args){	}` brackets, so that it isn't automatically executed, but executed 'on demand' instead. For example:

```java
package alex.appium.project0;

public class BrushUp1 {

	public static void main(String[] args) {

	}
	public void getData(){
        System.out.println("Hola");
    }
}
```

In the example above we created **getData() method** along with **'public' as access modifier, meaning that getData() can be accessed by other classes too**, and setting a 'void' return.



> ACCESSING METHODS IN MAIN BLOCK (FROM THE SAME CLASS)

As seen in previous examples, **we can't access a method directly, first, we need to create an object of the class the method belongs to**.

For example, to access 'getData()' method, we would need to create an object of the 'BrushUp1' class, and only then access the method, as follows:

```java
package alex.appium.project0;

public class BrushUp1 {

	public static void main(String[] args) {
        BrushUp1 myObject = new BrushUp1();
        myObject.getData();
	}
	public void getData(){
        System.out.println("Hola");
    }
}
```

In the case our method doesn't have a 'void' return, we would need another variable to store the result, like this:

```java
package alex.appium.project0;

public class BrushUp1 {

	public static void main(String[] args) {
        BrushUp1 myObject = new BrushUp1();			// class object
        String myUser = myObject.getData();			// variable to store method's return
        System.out.println(myUser);					// print method's return
	}
	public String getData(){
        System.out.println("Retornando el nombre del usuario:");
        return "Alex";
    }
}
```



> METHOD'S ONLY CLASSES

Up until now we've declared methods coexisting with a main block for execution, but that's not necessary if we don't want to. We can also create a **class for methods-only**.

```java
package alex.appium.project0;

public class methodsOnly {
	public String getData(){
        System.out.println("Retornando el nombre del usuario:");
        return "Alex";
    }
}
```



> ACCESSING METHODS FROM A DIFFERENT CLASS

Let's assume we need to access <u>getData()</u> method, which belongs to <u>methodsOnly</u> class, from <u>BrushUp1</u> class. In that case, and as mentioned earlier, we need to create an object of the class the method belongs to.

```java
package alex.appium.project0;

public class BrushUp1 {

	public static void main(String[] args) {
        methodsOnly myObject = new methodsOnly();	// class object
        String myUser = myObject.getData();			// variable to store method's return
        System.out.println(myUser);					// print method's return
	}
}
```



> ACCESS A METHOD WITHOUT AN OBJECT: STATIC METHODS

If we don't want to create an object of the class, we need to set our method as `static`.

```java
public static String getData(){
}
```

This means that n**ow the method is referenced to a class (the class it belongs to) and not to an object**.

So, static methods can be called without the need of an object. Being 'static' means that there's only one copy of the method in memory, regardless the amount of objects of the class.



> STATIC METHODS WITH CLASS LEVEL ACCESS

There are two ways of accessing a static method. The first one is this:

```java
package alex.appium.project0;

public class BrushUp1 {

	public static void main(String[] args) {
        String myUser = getData();
	}
    public static String getData(){
        return "Alex";
    }
}
```

We can see that we only need to write the name of the method and assign its return value to a variable. 

A clear **disadvantage** of this is that they're moved to class level access, meaning that they can't be accesed from other classes.



> STATIC METHODS: GLOBAL ACCESS

There's a second way to acess a static method, which is done via the class name, like this:

```java
package alex.appium.project0;

public class BrushUp1 {

	public static void main(String[] args) {
        String myUser = BrushUp1.getData();
	}
    public static String getData(){
        return "Alex";
    }
}
```

By writing the class name the method belongs to, we ensure the compiler is gonna find it, even if it's in a different class.

### Inheritance

Inheritance is a mechanisms that allows to create a new class from an existing class.

The new class, called a **derived class**, child class or subclass, inherits (gets) the attributes and methods of the original class, called **superclass**, parent class or base class.

This prevents from code duplication and simplifies maintainment.



> EXAMPLE

Let's say we have:

- Class: Animal
  - Attributes: nombre, especie, edad
  - Methods: comer, dormir

```java
public class Animal {

    private String nombre;
    private String especie;
    private int edad;

    public Animal(String nombre, String especie, int edad) {
        this.nombre = animalName;
        this.especie = animalSpecies;
        this.edad = animalAge;
    }

    public void comer() {
        System.out.println("El animal está comiendo");
    }

    public void dormir() {
        System.out.println("El animal está durmiendo");
    }

    // ...
}
```

(`this` is used to reference the actual object of the class; `.nombre`, `.especie` and `.edad` are access operators to the attribute they mention, and `animalName`, `animalSpecies` and `animalAge` are the values that we're assigning to the attributes - in fact variables with any values -)



Then, we could create a derived class named Perro which inherits from Animal class.

This subclass will have the methods and attributes defined in Animal class, but can have specific attributes and methods of dogs, like `Raza()` and `Ladrar()` aswell.

Then, we end up with this

- Subclass: Perro
  - Attributes: nombre, especie, edad, raza
  - Methods: comer, dormir, ladrar

```java
public class Perro extends Animal {

    private String raza;

    public Perro(String nombre, String especie, int edad, String raza) {
        super(nombre, especie, edad);
        this.raza = dogRace;
    }

    public void ladrar() {
        System.out.println("El perro está ladrando");
    }

    // ...
}
```

So **this can be seen as a way to add features using an exsisting class as a base**.



> SYNTAX TO DEFINE A CHILD CLASS

A child class can be defined this way:

```java
public class myChildClass extends myParentClass{
    
}
```


