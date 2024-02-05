# TROUBLESHOOTING

### Emulator doesn't start

> Error: 'Connecting to the emulator'

1. Clic on File -> Settings -> Tools -> Emulator
2.  Uncheck 'Launch in the Running Devices tool window'

Note: 

FTools -> Emulator -> Uncheck “Launch in a tool window”.



# INTRODUCTION

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

### Installation and Config

> DOWNLOADS AND ENV VARIABLES

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

   

> ANDROID STUDIO AND VIRTUAL DEVICE (EMULATOR)
>

1. Open AS

2. New project -> Basic Activity

3. Tools -> Device manager -> Create Virtual Device

4. **Device definition:** Select '**Pixel 7 Pro**'

5. **System image:** Select '**UpsideDownCake**' (android 14.0) from **x86 Images**.

   Note: It's important to select the image from 'x86 Images' tab and not from 'Recommended', since the last one won't allow to change RAM allocation.

6. **Verify config:** write an AVD name, e.g. 'Pixel 7 Pro API 34'. Here, I also set the RAM to **4096** and the emulated performance to '**Hardware**'.

7. Finish

8. Start the device (we'll see the phone)

> APPIUM SERVER

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

   

> ECLIPSE EDITOR
>

Eclipse Editor will be used to write our Appium code.

1. Download 'Eclipse IDE for Java Developers' from the web.

2. Open Eclipse

   The default workspace is:

   ```bash
   C:\Users\Diego\eclipse-workspaces
   ```

   

> JAVA CLIENT (FROM MAVEN)

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




> FRAMEWORK: TESTNG

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



> NEW PROJECT: ECLIPSE + MAVEN

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

   **GroupID** (kinda the package name): **alex.appium**

   **ArtifactId** (kinda the project name): **project0**

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

10. **TIP:** Verify that the 'Build automatically' option in Tools is checked, so there's a scan for problems everytime we modify and save a file. | This scan can also be done by right-clicking the name of our project and clicking on Maven -> Update project.

Now, in 'Maven Dependencies' at the left panel, we can see all the jars that both java-client and testng have automatically downloaded for us.



# Core Java basics for automation

### Variables and Data Types

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

   The whole execution will take place inside of the main block, which is inside the  ...args){	} brackets. Any code outside that block won't be executed.

4. s



s

# Arrays



# Loops and Conditions



# Strings



# Arrays lists



### Array lists operations and conversion of array to list



# Declaring Methods



# Accessing Methods in class and Static keyword

