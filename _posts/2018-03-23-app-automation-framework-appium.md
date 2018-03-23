---
layout:     post
title:      appium automation framework
subtitle:    "\"framework\""
date:       2018-03-23
author:     Tong
header-img: img/post-bg-2015.jpg
catalog: true
tags:
    - Python
---

"appium" is a popular automation framework based on mobile terminal.

You can download from its official website:

http://appium.io/



**environment set up**

Once install the package, u can  set up your environment as follows: 

```
> brew install node      # get node.js
> npm install -g appium  # get appium
> npm install wd         # get appium client
> appium &               # start appium
> node your-appium-test.js
```



**demo**

Below is a demo for iOS platform.

```python
"""An example of Appium running on Sauce.
This test assumes SAUCE_USERNAME and SAUCE_ACCESS_KEY are environment variables
set to your Sauce Labs username and access key."""

from random import randint
from appium import webdriver
from appium import SauceTestCase, on_platforms


platforms = [{
                'platformName': 'iOS',
                'platformVersion': '7.1',
                'deviceName': 'iPhone Simulator',
                'app': 'http://appium.s3.amazonaws.com/TestApp6.0.app.zip',
                'appiumVersion': '1.3.4'
            }]

@on_platforms(platforms)
class SimpleIOSSauceTests(SauceTestCase):

    def _populate(self):
        # populate text fields with two random numbers
        els = self.driver.find_elements_by_class_name('UIATextField')

        self._sum = 0
        for i in range(2):
            rnd = randint(0, 10)
            els[i].send_keys(rnd)
            self._sum += rnd

    def test_ui_computation(self):
        # populate text fields with values
        self._populate()

        # trigger computation by using the button
        self.driver.find_element_by_accessibility_id('ComputeSumButton').click()

        # is sum equal ?
        sum = self.driver.find_elements_by_class_name("UIAStaticText")[0].text
        self.assertEqual(int(sum), self._sum)

```

