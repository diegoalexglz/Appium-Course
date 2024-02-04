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

2. Download Android Studio, find out the Android SDK path (when installing) and add it to Environment System Variables.

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

Understand the Desired Capabilities to setup the Environment in the
Appium

