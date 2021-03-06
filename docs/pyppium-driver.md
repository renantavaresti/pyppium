# Pyppium Driver

The Pyppium driver is an abstraction to the conventional driver. He tries to simplify the driver usage and sharing instance in your test runner.

## Driver Basics

Pyppium driver has some facilities to use, and have some default parameters to simplify your tests. After start using the Pyppium remember to import it.

```python
from pyppium.driver import PyppiumDriver
```

### Connect to Appium Default Url 

If the Pyppium driver starts only, it will connect to ```"http://localhost:4723/wd/hub"```.

```python

Pyppium(caps)

```

!!! Hint
    You can read more about appium capabilities in this [Appium official documentation](http://appium.io/docs/en/writing-running-appium/caps/).

### Override Default Appium Url

You can override the Appium default URL.

```python

Pyppium("http://my-domain/wd/hub", caps)

```

### Driver Instance

After creating your driver instance you can access like that.

```python

Pyppium.instance()

```

This   `#!python instance()` call return a `#!python webdriver.Remote`. You can access attributes and functions as usual.

#### Screenshot

```python
PyppiumDriver.instance().get_screenshot_as_png()
```

#### Checking Presence of Keyboard

```python
PyppiumDriver.instance().is_keyboard_shown()
```
#### Session

```python

# Return session

PyppiumDriver.instance().session

# Return session_id

PyppiumDriver.instance().session_id
```

!!! Note
    All these commands are from `#!python webdriver.Remote`, for more hot moves, see [selenium official documentation](https://selenium-python.readthedocs.io/api.html).

## Getting Plataform Name

You can show the running platform name simple like that.

```python

from pyppium import driver

# Will return the name of the platform in lower case.
# android or ios

driver.platform_name()

```

## Checking Running Platform

```python

from pyppium import driver

driver.is_android()

# Or 

driver.is_ios()


```

## Quitting Driver

Before killing the driver instance pyppium only check if the driver instance is not `#!python None`.


```python
PyppiumDriver.quit()
```

## PyppiumDriver and Browserstack

You can connect pyppium with BrowserStack like this.

````python
PyppiumDriver(caps, user="meiko", keys="keys", use_browserstack=True)
````
Or you can use environment variables.

```

export BROWSERSTACK_USERNAME=my-user
export BROWSERSTACK_ACCESS_KEY=my-keys

```

And you don't need to use user and keys parameters anymore.

````python
PyppiumDriver(caps, use_browserstack=True)
````

Change between local appium and BrowserStack is easy, just changing the flag `#!python False`.

````python
PyppiumDriver(caps, user="meiko", keys="keys", use_browserstack=False)
````



!!! Warning
    Remember! BrowserStack have specific capabilities, you can check this in [Browserstack official documentation](https://www.browserstack.com/app-automate/capabilities). 


## Pytest Real Life Sample

If your application has the same behaviours, you can easily run a test for android and ios with only one test.

First, create your capabilities for android and ios.

```python

# Android appium capabilities

caps_android = {
    "platformName": "Android",
    "automationName": "uiautomator2",
    "deviceName": "Android Emulator",
    "appPackage": "com.example.dummy",
    "appActivity": "MainActivity",
    "newCommandTimeout": 0,
    "app": abspath("tests/e2e/apps/app-debug.apk")
}

# iOS appium capabilities

caps_ios = {
    "platformName": "iOS",
    "automationName": "xcuitest",
    "deviceName": "iPhone 8",
    "platformVersion": "13.6",
    "app": abspath("tests/e2e/apps/dummy.app")
}

```

Create your test case using parametrize and happy testing.

```python

@pytest.mark.parametrize("capabilities", [caps_ios, caps_android])
def test_should_show_welcome_message(capabilities):
    PyppiumDriver(capabilities)

    # You screens stuff...

    PyppiumDriver.quit()

```

!!! Warning
    Remember to create your screen using pyppium [fetcher]("https://leomenezessz.github.io/pyppium/fetcher/").


<br/>