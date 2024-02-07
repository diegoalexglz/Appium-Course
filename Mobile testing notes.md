# STARTING LECTURES DEPENDENCIES

We'll start with the following versions:

- Android Studio 2023.1.1.28
- Appium 2.4.1
- Java-Client 9.1.0
- TestNG 7.9.0
- Maven Archetype template 1.4

We **won't include selenium dependencies** as of now in the project, these packages will be installed later. So, the current POM.xml should be like this:

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

(Note: selenium 4.17.2 was installed and then uninstalled from python, to check that the latest version is 4.17.2)



# ADD COURSE'S APP TO PROJECT

The first app we'll use is the **myDemo.apk**. To place it into our workspace, we'll follow these steps:

1. Right click on <u>src/test/java</u>
2. New -> Package
3. Name the package: in this case, we'll name it resources'
4. Click on finish
5. Go to windows explorer and copy <u>myDemo.apk</u> file
6. Click on the <u>resources</u> package and paste the app with Ctrl + V.



# UI AUTOMATOR

UIAutomator was designed by Google to facilitate automation. Appium took this tool and came up with UIAutomator2, a modified version of the original framework.

### Check for Appium installed drivers

1. In powershell, paste:

   ```bash
   appium driver list
   ```

   The original list returned should be:

   Listing available drivers:
   - uiautomator2 [not installed]
   - xcuitest [not installed]
   - mac2 [not installed]
   - espresso [not installed]
   - safari [not installed]
   - gecko [not installed]
   - chromium [not installed]

### Install UIAutomator

1. In powershell, paste:

   ```bash
   appium driver install uiautomator2
   ```

2. Paste previous command (check for installed drivers) to verify if uiautomator2 was succesfully installed.

   In our case, we installed uiautomator2 version **2.44.0**



### TESTNG Basic class

1. Open Eclipse project
2. Right-click <u>alex.appium.project0</u> under <u>src/test/java</u>.
3. Create new class 'appiumBasics'

The simplest form of TestNG is:

```java
package alex.appium.project0;

import org.testng.annotations.Test;

public class appiumBasics {
	@Test
	public void myAppiumTest() {
		// Code to be executed
	}
}
```

We'll run this test as testng test, not as a java app anymore, so the shortcut will be:

​					Run as TestNG test

----------------------------   **ALT SHIFT X, N**   ----------------------------



### Android Object

> CREATE ANDROID OBJECT

1. **Prelim step: Open Appium server** via Powershell.

   ```bash
   appium
   ```

2. **Import needed packages:**

   ```java
   import org.testng.annotations.Test;
   import io.appium.java_client.android.AndroidDriver;
   import java.net.URL;
   import java.net.MalformedURLException;
   import io.appium.java_client.android.options.UiAutomator2Options;
   ```

   Here's the function of all of them:

   `org.testng.annotations.Test` :  identifies the method as a test case to be executed in testing.

   `io.appium.java_client.android.AndroidDriver` : allows to create Android object.

   `java.net.URL;import java.net.MalformedURLException` : allows to introduce Android-object's  IP and port.

   `io.appium.java_client.android.options.UiAutomator2Options` : allows to configure Android-object's capabilities.

3. **Create Android Object:**

   We need to tell Appium which driver we'll use. For this, we'll create an object of an Android driver class. This object needs two arguments (which appear as null at first):

   - Appium Service

   - Capabilities

   ```java
   AndroidDriver myDriver = new AndroidDriver(null,null);
   ```

4. **First argument: Appium Service**

   By default, server opens on port 4723 of our local IP addres (notice that we can't directly put the string, but he have to create a 'URL' class.).

   ```java
   AndroidDriver myDriver = new AndroidDriver(new URL("http://127.0.0.1:4723"), null);
   ```

5. **Second argument: Capabilites (create UiAutomator object)** 

   Capabilities refer to which operating system and particular device the test will take place, aswell as to which app we'll automate. To specify capabilities, we'll create an object of UiAutomator class.

   ```java
   UiAutomator2Options myOptions = new UiAutomator2Options();
   ```

   After that, we'll set the device with `.setDeviceName()` and `.myOptions.setApp()` methods.

   *IMPORTANT*	The **device name must match with the one** we chose when creating our AVD **in Android Studio**. So, for this case, it will be:

   ```java
   myOptions.setDeviceName("Pixel 7 Pro API 34");
   ```

   To find the

   ```java
   myOptions.setApp
   ```

   s

6. So, the whole picture looks like this:

   ```java
   package alex.appium.project0;
   
   import org.testng.annotations.Test;
   import io.appium.java_client.android.AndroidDriver;
   import io.appium.java_client.android.options.UiAutomator2Options;
   import java.net.URL;
   import java.net.MalformedURLException;
   
   public class appiumBasics {
   	@Test
   	public void myAppiumTest() throws MalformedURLException {
   		UiAutomator2Options myOptions = new UiAutomator2Options();
   		myOptions.setDeviceName("Pixel 7 Pro API 34");
   		
   		AndroidDriver myDriver = new AndroidDriver(new URL("http://127.0.0.1:4723"), myOptions);
   	}
   }
   ```



### Start Appium server with object (not via powershell)

> SERVER OBJECT

 Using the code above as a starting point, Appium server can be started with an object of `AppiumServiceBuilder()` class.

```java
AppiumServiceBuilder myService = new AppiumServiceBuilder();
```


> SERVER OBJECT PARAMETERS

Then, we use `.withAppiumJS()` method to reference the object towards <u>main.js</u>, which is the file that invokes appium server. 

```java
myService.withAppiumJS(new File("C://Users//Diego//AppData//Roaming//npm//node_modules//appium//build//lib//main.js"));
```

And then again, we use `.withIPAdress()` method to reference the object to our IP.

```java
myService.withIPAddress("http://127.0.0.1");
```

And `.usingPort()` method for our port.

```java
myService.usingPort(4723);
```



Notice how we also needed to create an object of 'File' class, and we didn't simply put the path as a String in  `.withAppiumJS()` method, but we did straightforward wrote the IP as a string in `.withIPAdress()` method, and that's because **every method tell us which data-type they need when we hover the cursor above them**.

NOTE: Java doesn't accept single slashes (/) in paths, they have to be double slashes (//).



> SERVER BUILD AND START

After setting up the object with the described methods above, we have to create a new object of `AppiumDriverLocalService` class and make use of `.buildService()` and `.start()` methods **to start the server**.

```java
AppiumDriverLocalService myServer = AppiumDriverLocalService.buildService(myService);
myServer.start();
```

The new packages added are:

```java
import java.io.File;
import io.appium.java_client.service.local.AppiumServiceBuilder;
import io.appium.java_client.service.local.AppiumDriverLocalService;
```

The whole picture now is:

```java
package alex.appium.project0;

import org.testng.annotations.Test;
import io.appium.java_client.android.AndroidDriver;
import io.appium.java_client.android.options.UiAutomator2Options;
import java.net.URL;
import java.net.MalformedURLException;
import java.io.File;
import io.appium.java_client.service.local.AppiumServiceBuilder;
import io.appium.java_client.service.local.AppiumDriverLocalService;

public class appiumBasics {
	@Test
	public void myAppiumTest() throws MalformedURLException {
		AppiumServiceBuilder myService = new AppiumServiceBuilder();
		myService.withAppiumJS(new File("C://Users//Diego//AppData//Roaming//npm//node_modules//appium//build//lib//main.js"));
		myService.withIPAddress("http://127.0.0.1:4723");
		myService.usingPort(4723);
		
		AppiumDriverLocalService myServer = AppiumDriverLocalService.buildService(myService);
        myServer.start();

		UiAutomator2Options myOptions = new UiAutomator2Options();
		myOptions.setDeviceName("Pixel 7 Pro API 34");
		
		AndroidDriver myDriver = new AndroidDriver(new URL("http://127.0.0.1:4723"), myOptions);
	}
}
```



### s