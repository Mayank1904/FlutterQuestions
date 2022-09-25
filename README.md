# FlutterQuestions

A. Descriptive Questions:
1. Can we nest the Scaffold widget? Why or Why not?

Yes, We can nest the Scaffold widget. Scaffold is a widget so we can place it anywhere where it fits. By nesting the Scaffold we can layer drawers, snack bars and bottom sheets.

But there should be one Scaffold for each page/screen and that it should not nest Scaffolds.

Example:

import 'package:flutter/material.dart';

void main() {
  runApp(MaterialApp(
    title: 'Navigation Basics',
    home: FirstRoute(),
  ));
}

class FirstRoute extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('First Route'),
      ),
      body: Center(
        child: ElevatedButton(
          child: const Text('Open route'),
          onPressed: () {
            Navigator.push(
              context,
              MaterialPageRoute(builder: (context) => SecondRoute()),
            );
          },
        ),
      ),
    );
  }
}

class SecondRoute extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text("Second Route"),
      ),
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            Navigator.pop(context);
          },
          child: const Text('Go back!'),
        ),
      ),
    );
  }
}

Here, I can see there are two distinct screen and each one has its own Scaffold. We are navigating back and forth by using Navigator, and so our pages are being added to the stack, one Scaffold on top of the other.

On the other hand,  If I want to navigate inside the body of a Scaffold so I am changing the app bar depending on the content using TabControllers, such as DefaultTabController, is preferred. 

import 'package:flutter/material.dart';

void main() {
  runApp(TabBarDemo());
}

class TabBarDemo extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: DefaultTabController(
        length: 3,
        child: Scaffold(
          appBar: AppBar(
            bottom: const TabBar(
              tabs: [
                Tab(icon: Icon(Icons.directions_car)),
                Tab(icon: Icon(Icons.directions_transit)),
                Tab(icon: Icon(Icons.directions_bike)),
              ],
            ),
            title: const Text('Tabs Demo'),
          ),
          body: const TabBarView(
            children: [
              Icon(Icons.directions_car),
              Icon(Icons.directions_transit),
              Icon(Icons.directions_bike),
            ],
          ),
        ),
      ),
    );
  }
}

As a general rule of thumb: use only one Scaffold per screen. Use only one Scaffold with widgets such as TabController or IndexedStack to navigate the content inside the body of a single screen.

2. What are the different ways we can create a custom widget ?

The first is to refactor a large widget tree into smaller individual widgets, each with its own build method.
The normal way to create widgets in Flutter is through composition, which means combining several basic widgets into a more complex one.


3. How can I access platform(iOS or Android) specific code from Flutter?

Method channels are platform channels designed for invoking named pieces of code across Dart and Android or iOS. Method channels make use of standardized message to convey method name and arguments from sender to receiver, and to distinguish between successfull and erroneous results in the associated reply.

Like We want to use some sdk lets say Truecaller that not be available in flutter so We will write the native code and call the native code using Method Channels.

To receive method calls on Android, you need the channel’s name and a closure. The name is like an ID for the channel and must be the same for iOS, Flutter and Android. The Flutter Platform Engine calls the closure when you invoke a method on the Method Channel from the Dart side. In this closure, you retrieve the name and parameters of the invoked method and consume it.

4. What is BuildContext? What is its importance?

As we all know that in Flutter, everything is a widget. A text, an image, a column, a row, etc everything that you see on the Flutter application is a widget and a parent widget can have one or more child widgets also. The whole widgets of a Flutter application are displayed in the form of a Widget Tree where we connect the parent and child widgets to show a relationship between them. Every widget is having someplace in the widget tree lets say the Scaffold widget is in the MaterialApp widget.
So, here comes the role of BuildContext. The BuildContext is used to locate a particular widget in a widget tree and every widget has its own BuidContext i.e. a particular BuildContext is associated with only one widget.
As we know that every widget in Flutter is created by the build method and the build method takes a BuildContext as an argument. This helps the build method to find which widget it is going to draw and also it helps in locating the position of the widget to be drawn in the widget tree.
Also, one thing to note here is that widgets are only visible to their BuildContext or to its parent's BuildContext. So, from a child widget, you can locate the parent widget.


B. Coding Questions:

1. Refactor the code below so that the children will wrap to the next line when
the display width is small for them to fit.

Row will be replaced by Wrap Widget

![image](https://user-images.githubusercontent.com/16149915/192161182-40e3565b-9abd-4092-9d3f-825e9088c4d8.png)


2.Identify the problem in the following code block and correct it.

It will block the app as it is computationally expensive task.

Making it an async function wouldn't help, either, because it would still run on the same isolate.

![image](https://user-images.githubusercontent.com/16149915/192158762-9f61ff76-e3e0-4e48-b8a9-e025c47cb5af.png)

The solution is to run it on a different isolate:

Future<String> longOperationMethodIsolate() async {
  return await compute(longOperationMethod, 1000000000);
}

String longOperationMethod(int countTo) {
  var counting = 0;
  for (var i = 1; i <= countTo; i++) {
    counting = i;
  }
  return '$counting! Ready or not, here I come!';
}


3. In the below code, list1 declared with var, list2 with final and list3 with const.
What is the difference between these lists? Will the last two lines compile?

void main() {
  var list1 = ['I', 'am', 'mayank'];
  final list2 = list1;
  list2[2] = 'Dart'; // This line will compile without error
  const list3 = list1; // This line will give error because const variables must be initialized with a constant value.
}

![image](https://user-images.githubusercontent.com/16149915/192159130-39482a95-390e-408b-a77c-c6e4665591d4.png)

Here's the difference:

final - means you can't use the operator =, But you can add, or remove items. The assignment is final, however, the values are NOT constant.
const - means you can't re-assign nor change the values.
