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

The first app we'll use is the **myDemo.apk**. To place it into our Eclipse's project, we'll follow these steps:

1. Right-click on <u>src/test/java</u>
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

> CREATE ANDROID OBJECT (TO START APP)

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

   `java.net.URL` : allows to introduce Android-object's  IP and port.

   `java.net.MalformedURLException` : allows to introduce URL.

   `io.appium.java_client.android.options.UiAutomator2Options` : allows to configure Android-object's capabilities.

3. **Create Android Object:**

   We need to tell Appium which driver we'll use. For this, we'll create an object of an Android driver class. This object needs two arguments (which appear as null at first):

   - Appium Service

   - Capabilities

   ```java
   AndroidDriver myDriver = new AndroidDriver(null,null);
   ```

4. **First argument: Appium Service**

   By default, server opens on port 4723 of our local IP address (notice that we can't directly put the string, but he have to create a 'URL' class.).

   ```java
   AndroidDriver myDriver = new AndroidDriver(new URL("http://127.0.0.1:4723"), null);
   ```

5. **Second argument: Capabilites (create UiAutomator object)** 

   Capabilities refer to which operating system and particular device the test will take place, aswell as to which app we'll automate. To specify capabilities, we'll create an object of UiAutomator class.

   ```java
   UiAutomator2Options myOptions = new UiAutomator2Options();
   ```

   After that, we'll set the device with `.setDeviceName()` and `.setApp()` methods.

   ```java
   myOptions.setDeviceName("Pixel 7 Pro API 34");
   myOptions.setApp("C://Users//Diego//eclipse-workspace//project0//src//test//java//resources//myDemo.apk");
   ```

   *IMPORTANT*

   - The **device name must match with the one** we chose when creating our AVD **in Android Studio**.
   - The **app path** can be found by right-clicking on <u>myDemo.apk</u> (inside src/test/java -> alex.appium.project0 -> resources), clicking on 'propierties' and then copying the location.



> CLOSE APP (UIAUTOMATOR SESSION)

We use `.quit()` method to disconnect the driver (UIAutomator2) from Appium server.

```java
myDriver.quit();
```




#### First 'runnable' test

At this point, we can execute our first test:

1. Open AS, run AVD
2. Open Appium server via powershell
3. Open Eclipse, run the code below as 'TestNG Test'

The whole picture up until now looks like this. When run, it will install the <u>myDemo.apk</u> app to our AVD.

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
        myOptions.setApp("C://Users//Diego//eclipse-workspace//project0//src//test//java//resources//myDemo.apk");

        AndroidDriver myDriver = new AndroidDriver(new URL("http://127.0.0.1:4723"), myOptions);
        
        myDriver.quit();
    }
}
```

And it's a success!

<img src="images/im_01.png" width="300" style="float: left;">

#### First ever logs

In powershell, we can see everything appium did when we ran the test in eclipse. Some important logs are these:

- **Login and environment config:**
  - <u>[Appium]</u> AppiumDriver.createSession() -> New appium session
  - <u>[Appium]</u> Event 'newSessionRequested' logged -> New session request
  - <u>[Appium]</u> Attempting to find matching driver -> Looks for proper driver (UIAutomator in this case)
  - <u>[Appium]</u> Session created with session id -> shows the session's id
  - <u>[BaseDriver]</u> Using local app 'C://Users//Diego//eclipse-workspace/...' -> app to be tested
- **Connection with device:**
  - <u>[UIAutomator2]</u> Retrieving device list -> Looks for connected devices
  - <u>[ADB]</u> Connected devices: [{"ud...  -> Connected device (an emulator, in this case)
  - <u>[UIAutomator2]</u> Using device: emulator-5554 -> Confirms the device
- **Obtaining device info:**
  - [<u>ADB]</u> Current device property 'ro.build.version.sdk': 34 -> Device's SDK Version
  - <u>[ADB]</u> Current device property 'ro.build.version.release': 14 -> Operating system (android) version
- **Configuring Appium apk:**
  - <u>[UIAutomator2]</u> Pushing settings apk to the device... -> Appium's config app installation into the device
  - <u>[ADB]</u> The installation of 'settings_apk-debug.apk' took 197ms -> Installation time
- **Starting UiAutomator2 server:**
  - <u>[ADB]</u> Forwarding system: 8200 to device: 6790 -> Sends AVD's 6790 port to local's 8200 port for UIAutomator communication
- **App's certificates verification:**
  - <u>[ADB]</u> Checking app cert for C://...myDemo.apk -> Validates app-to-be-tested's certificates
- **Application installation:**
  - <u>[ADB]</u> Installing 'C://...myDemo.apk' -> Installing app to be testes
- **UIAutomator2 server startup:**
  - <u>[UIAutomator2]</u> Starting UIAutomator2 server 6.0.7 -> UIAutomator 6.0.7 starts
- **Creating automation session:**
  - [UIAutomator2] Matched '/session' to command name 'createSession'
- **Deleting session:**
  - **[UIAutomator2]** Matched '/' to command name 'deleteSession'

- **Instrumentation:**
  - <u>[Instrumentation]</u> Time: 64.207 -> Test time
    <u>[Instrumentation]</u> OK (1 test)
    <u>[Instrumentation]</u> The process has exited with code 0



Glossary:

ADB (Android Debug Bridge)



### Start and Close Appium server with object (not via powershell)

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



> CLOSE THE SERVER

We use `.quit()` method to close the service.

```java
myServer.close();
```



#### Second 'runnable' test

At this point, we only need to:

1. Open Eclipse and run the code below as 'TestNG Test'.

(No need of using powershell)

When run, it will open <u>myDemo.apk</u> app in our AVD.

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
		
        // ECLIPSE CODE -> APPIUM SERVER -> ANDROID STUDIO
		// Appium server config
		AppiumServiceBuilder myService = new AppiumServiceBuilder();
		myService.withAppiumJS(new File("C://Users//Diego//AppData//Roaming//npm//node_modules//appium//build//lib//main.js"));
		myService.withIPAddress("http://127.0.0.1:4723");
		myService.usingPort(4723);
		
		// Apium server startup
		AppiumDriverLocalService myServer = AppiumDriverLocalService.buildService(myService);
        myServer.start();

        // Android object Capabilities (device and app specs)
		UiAutomator2Options myOptions = new UiAutomator2Options();
		myOptions.setDeviceName("Pixel 7 Pro API 34");
        myOptions.setApp("C://Users//Diego//eclipse-workspace//project0//src//test//java//resources//myDemo.apk");
		
        // Android object and app (session) startup
		AndroidDriver myDriver = new AndroidDriver(new URL("http://127.0.0.1:4723"), myOptions);
		
		// App (session) closing and UIAutomator2 disconnection
		myDriver.quit();
		
		// Appium server closing
		myServer.stop();
        
        // Actual automation goes here...
	}
}
```

Now the logs will be shown in Eclipse's console.

NOTE: Also, the app gets stored and available in the AVD.

With this, we've finished the part of 'One time effort'. Next, comes the automation.



# APPIUM INSPECTOR

**To select (click) an item of an app, we need to know its properties**, so we can define the type of locator we should use (Xpath, id, accessibilityId, classname or androidUIAutomator). To this purpose, there is <u>Appium inspector</u>.

### Installation

1. Go to 

s