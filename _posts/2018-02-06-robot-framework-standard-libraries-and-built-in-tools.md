---
layout:     post
title:      robot framework standard libraries and built-in tools
subtitle:    "test framework"
date:       2018-02-06
author:     Tong
header-img: img/post-bg-2015.jpg
catalog: true
tags:
    - Python
    - robot
    - framework
---

Robot Framework is a useful automation framework for QA daily work.

It contains standard library and built-in tools.

##### Standard libraries

|     Library     |               Introduction               |
| :-------------: | :--------------------------------------: |
|     BuiltIn     | Contains generic often needed keywords. Imported automatically and thus always available. |
|   Collections   | Contains keywords for handling lists and dictionaries. |
|    DateTime     | Supports creating and verifying date and time values as well as calculations between them. |
|     Dialogs     | Supports pausing the test execution and getting input from users. |
| OperatingSystem | Enables performing various operating system related tasks. |
|     Process     | Supports executing processes in the system. |
|     Remote      | Part of the [remote library interface](https://github.com/robotframework/RemoteInterface). Does not have any keywords of its own. |
|   Screenshot    | Provides keywords to capture and store screenshots of the desktop. |
|     String      | Library for manipulating strings and verifying their contents. |
|     Telnet      | Supports connecting to Telnet servers and executing commands on the opened connections. |
|       XML       | Library for verifying and modifying XML documents. |

##### Built-in tools

|  Tool   |               Introduction               |
| :-----: | :--------------------------------------: |
|  Rebot  | Tool for generating logs and reports based on XML outputs and for combining multiple outputs together. |
| Libdoc  | Tool for generating keyword documentation for test libraries and resource files. |
| Testdoc | Generates high level HTML documentation based on Robot Framework test cases. |
|  Tidy   | Tool for cleaning up and changing format of Robot Framework test data files. |