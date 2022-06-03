# Passing By Value vs Passing by Reference

## Learning Goals

- Explain the difference between passing by value and reference

## Introduction

When passing parameters to methods, programming languages either pass parameters
by `value` or by `reference`.

First let's review what a `reference` and a `value` are in this context:

![object-reference.png](https://curriculum-content.s3.amazonaws.com/java-mod-1/passing-by-value-vs-reference/module-1-object-reference.png)

A illustrated above, a `reference` is what we can use in order to access an
object's values in memory. With that in mind, let's consider these definitions:

- Passing a parameter by `reference` means that the receiving method is getting
  the same object as the calling method passed in
- Passing a parameter by `value` means that the receiving method is getting a
  copy of the object that the calling method passed in

In reality, and in Java, it's not quite that simple. Let's consider the
following two classes as an example:

```java
public class ContainerClass {
    public int number;
    public String text;
}
```

```java
public class SampleClass {
    public static void main(String[] args) {
        ContainerClass myObject = new ContainerClass();
        myObject.number = 0;
        addNumbers(myObject, 12, 7);
        System.out.println("sum = " + myObject.number);
    }

    public static void addNumbers(ContainerClass container, int firstNumber, int secondNumber) {
        container.number = firstNumber + secondNumber;
    }
}
```

In Java, object references are passed by value. This means that the
`addNumbers()` method in the example above is getting its own copy of the
container object reference, and that object reference is pointing to the same
object as the calling method, as illustrated here:

![two-references-same-object.png](https://curriculum-content.s3.amazonaws.com/java-mod-1/passing-by-value-vs-reference/module-1-two-references-same-object.png)

This is a bit convoluted, so let's break it down:

- The `main()` method create an object of type `ContainerClass` and calls it
  `myObject`.
- `myObject` is a reference to a place in the computer's memory where the data
  for that object exists. Any time we want to recall that object, we can use
  that reference and we can use `dot notation` to access any of its data.
- The `addNumbers()` method receives a copy of the `myObject` reference and
  chooses to call it `container` - we could have called that reference anything
  we wanted, including `myObject` and it would a) compile and b) not impact of
  any of the behavior we are describing here.
- The `container` reference is a copy of the `myObject` reference. They both
  start with same value, which is a reference to the place in memory where the
  data for `myObject` lives.
- When I use the `dot notation` with the `myObject` reference in the `main()`
  method, I'm accessing the same data as when I use the `dot notation` with the
  `container` reference in the `addNumbers()` method.
- This is why running the code above will display that the value of
  `myObject.number` in the `main()` method is `19`, even though that value was
  changed in the `addNumbers()` method through the `container` reference. In
  other words, `myObject.number` in `main()` and `container.number` in
  `addNumbers()` are the same thing.

So far, so good: objects are references, and references are passed by value, so
when a change is made _inside_ of a reference by the receiving method, the
calling method will see that change.

Now consider the following modified code for the `addNumbers()` method:

```java
    public static void addNumbers(ContainerClass container, int firstNumber, int secondNumber) {
        container = new ContainerClass(); // this is the line we added
        container.number = firstNumber + secondNumber;
    }
```

What do you think the new output for the program is with this new version of the
`addNumbers()` method? Now go ahead and run it and observe it for yourself.

It now outputs `0` as a the result of the sum, and here is why:

- `addNumbers()` receives a copy of the `myObject` reference, as it did in the
  original version
- But now the first thing it does is take that reference, called `container` and
  use the assignment operator `=` to assign it a different value
- And since the reference is passed by value, this new value that it is assigned
  inside of `addNumbers()` does not have any impact on the original value of
  `myObject` inside of `main()`
- So now we have two references, as before, but they now point to two different
  objects in memory, which is why changes that are made through one reference
  are not reflected through the other reference anymore.

Here is an illustration of what has happened with this new code:

![two-references-two-objects.png](https://curriculum-content.s3.amazonaws.com/java-mod-1/passing-by-value-vs-reference/module-1-two-references-two-objects.png)

There are two exceptions to the statement that Java passes parameters by `value`
of `references`:

1. All primitive types are passed by value, meaning that changes to a variable
   of a primitive type inside a method never have an impact on that variable
   outside that method
2. All immutable types are also passed by value, with the same implication as
   above
