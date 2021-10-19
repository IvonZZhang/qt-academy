---
layout: lecture
title: QObject - a C++ object enhanced by Qt
ready: false
---

Qt makes C++ more powerful and easier to use, and the way to achieve that is **QObject**.

## Why it's easier to use?

If you browse the documentation of Qt, you'll find out that Qt provides a lot of modules like QtCore, QtNetwork, QtWidget... These modules are set of classes which you could use as regular C++ classes. For example, in QtCore, there are container classes like QVector, QList, QMap etc. These containers can do what STL containers could do but also with other features e.g. they support both STL-style iterators (with `begin()` and `end()`) and Java-style iterators (with `it.hasNext()` and `it.next()`). There are also classes like QThread, QMutex, QFuture etc. which makes it much easier to do concurrent and asychronous programming in C++ in a safe way.

A big part that everyone wouldn't miss is the GUI part. In QtWidget module, there are classes like QComboBox, QTextEdit, QFrame... with fully configurable parameters. You could design a GUI application just by creating and configuring these classes in C++, you could also visually and interactively do this via the Qt Designer tool. There is even a easier and more modern way to do it - via QML, a declarative language which you'll feel at home if you have some front-end experience.

The most powerful bit of Qt, is the mechanism to connect objects, called **signals and slots**. Signals mean "something happened", and "Slots" mean "a kind of reaction". E.g. I have a GardenCamera object that has a signal `startedRaining()` and I have a HouseManager object that has a slot `closeWindows()`. They don't know the existence of each other, but I could connect the signal and the slot so that whenever the signal `startedRaining()` is emitted, the slot `closeWindows()` is executed. The GardenCamera simply emits the signal, and that's it. It doesn't know whether there is an object that listens and responds to it, or how many objects are listening. I could also connect e.g. the slot `closeDoor()` of my Garage object to the same signal, with GardenCamera and HouseManager being totally not aware.

**Signals/slots** is very useful between the GUI objects and the objects handling the business logic. Each QWidget has a set of pre-defined signals. E.g. QComboBox has a `currentIndexChanged(int index)`, which is emitted whenever the user changes the selected item in the combobox. You could simply define what should be done once an item has been selected in a slot, and connect it with that signal. The business logic is then well defined for GUI interactions. In addition, in case you have noticed, the signal has a parameter `int index`. It's also possible to pass parameters through Signal/slots.

```c++
# Aforementioned example is as simple as this
GardenCamera gardenCamera;
HouseManager houseManager;
QObject::connect(&gardenCamera, &GardenCamera::startedRaining, &houseManager, &HouseManager::closeWindows);
```

