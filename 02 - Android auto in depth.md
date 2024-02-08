# BROWSE AN APP IN INSPECTOR

> OPTION A: STRAIGHTFORWARD FROM INSPECTOR

We've seen the three main sections of an Inspector session: **preview (or screenshot)**, **app source**, and **selected element**.

In preview, we can touch any element we want and see its properties in selected element. And there, we also have some actions available, **namely**:

- Tap (it's a click on the item)
- Send keys
- Copy attributes to clipboard
- Get timing

<img src="images/im_03.png" width="500" style="float: left;">



> OPTION B: FROM ANDROID STUDIO

A second way is to browse the app in Android Studio and go to the page we want, then, come back to Inspector and select **'Refresh source and screenshot'** option, from the options bar at the top.

<img src="images/im_04.png" width="350" style="float: left;">



# LOCATORS

### Common Xpath syntax

An Xpath is made using this structure: `//tagName[@attribute='value']`, whereas its actual properties are defined in <u>XML</u> code.

This implies that **we can make the Xpath by ourselves just by looking at the XML description of the element** and following the syntax above (in the case the Xpath wasn't recognized by Inspector).

### Correct use of By.xpath()

> NO BACKSLASHES

When copying an Xpath, we must ensure that it **doesn't contain backslashes** `\`. When they contain backslashes, we must remove them, like in this example: 

- This is wrong ❎ `//android.widget.TextView[@content-desc=\'3. Preference dependencies\']`

- This is correct ✅ `//android.widget.TextView[@content-desc='3. Preference dependencies']`



> NO DOUBLE QUOTES

Also, it **shouldn't have double quotes** `""`. If it has, we must change them for single quotes `''`, like in this example:

* This is wrong ❎ `//android.widget.TextView[@content-desc="3. Preference dependencies"]`.

- This is correct ✅`//android.widget.TextView[@content-desc='3. Preference dependencies']`.

This needs to be done because, when pasted as an argument of By.xpath(), the string will be enclosed between double quotes.



### Test 1

We wanna do the following:

GOALS

1. Open API Demos (myDemo.apk)
2. Tap on 'Preferences'
3. Tap on '3. Preference dependencies'

s
