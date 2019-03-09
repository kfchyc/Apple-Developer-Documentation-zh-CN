#Article

# Creating a Core Data Model

Create a data model file to contain your app’s object structure.

## Overview

The first step in working with Core Data is to create a data model file. Here you define the structure of your application’s objects, including their object types, properties, and relationships.

You can add a Core Data model file to your Xcode project when you create the project, or you can add it to an existing project.

## Add Core Data to a New Xcode Project
In the dialog for creating a new project, select the Use Core Data checkbox.

![](https://ws3.sinaimg.cn/large/006tKfTcly1g0pwkn2ei6j311s0re41b.jpg)

The resulting project includes an `.xcdatamodeld` file.

![](https://ws2.sinaimg.cn/large/006tKfTcly1g0pwlal8wwj311s0g6mzw.jpg)

### Add a Core Data Model to an Existing Project

Choose File > New > File and select from the iOS templates. Scroll down to the Core Data section, and choose Data Model:

![](https://ws3.sinaimg.cn/large/006tKfTcly1g0pwm6y1cej311s0remzw.jpg)

Click Next. Name your model file, and select its group and targets.

![](https://ws3.sinaimg.cn/large/006tKfTcly1g0pwmxf12vj311s0kfmze.jpg)

An `.xcdatamodeld` file with the name you specified is added to your project:

![](https://ws3.sinaimg.cn/large/006tKfTcly1g0pwndcmzyj311s0ga76z.jpg)