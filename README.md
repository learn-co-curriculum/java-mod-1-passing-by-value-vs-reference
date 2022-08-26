# Passing By Value vs Passing by Reference

## Learning Goals

- Explain the difference between passing by value and reference

## Introduction

When passing parameters to methods, programming languages either pass parameters
by value or by reference. But what does this mean?

Consider this scenario: Suzie and Dustin are students. Dustin wants to borrow
Suzie's notes to study for an upcoming test. Since Suzie would do anything to
help Dustin, she considers two options:

- Option 1: She gives Dustin her original notes.
- Option 2: She gives Dustin a photocopy of her notes.

If Suzie chooses option 1, then she provides her original notes to Dustin.
Dustin could then potentially alter her notes. If he does, then when he returns
those notes to Suzie, her notes have been changed.

If Suzie chooses option 2, then she keeps her original notes and provides
Dustin a copy of her notes. If Dustin makes modifications to the copy and
returns that to Suzie, her original notes remain unchanged.

This scenario is actually an analogy to show the differences between pass by
reference versus pass by value. Option 1 would represent pass by reference and
option 2 would represent pass by value.

**Pass-by-reference** (also known as call-by-reference) is when we pass the
same object as a parameter to a receiving method. When a parameter is passed by
reference, the receiving method can then access the parameter's value directly
and make modifications to it. Just like with option 1, Dustin would then have
direct access to the original notes and can make changes to them.

**Pass-by-value** (also known as call-by-value) is when we pass a copy of the
object as a parameter to the receiving method. When a parameter is passed by
value, a _copy_ of the parameter's value is actually passed to the receiving
method. The receiving method exclusively works with only a copy and therefore
the changes made to the argument will not affect the original variable's value.
Relating this back to our scenario, this would be the same situation as option
2 where Suzie provides only a copy of her notes instead of handing over her
original notes.

## Java is Pass-By-Value

In Java, all arguments are pass-by-value. Java does not allow parameters to be
passed by reference, meaning objects themselves cannot be passed around to
different methods.

Let's look at a very simple example to show how pass-by-value works:

```java
public class Example {
    public static void main(String[] args) {
        int number = 10;
        System.out.println("number before method call is " + number);
        change(number);
        System.out.println("number after method call is " + number);
    }

    public static void change(int numberCopy) {
        numberCopy = numberCopy + 2;
        System.out.println("numberCopy in the method call is " + numberCopy);
    }
}
```

- When the code is first executed, we will enter the `main()` method and
  initialize `number` to the value of 10.
- We will then output the value of `number` before calling the `change()`
  method.
- In the `change()` method, we are passing it `number`. But remember - Java is
  pass-by-value only. So this means that a copy of `number` is passed to the
  method `change()`.
- The code now enters in the `change()` method and receives a copy of `number`.
  The receiving method chooses to call the copy of `number` `numberCopy`. Notice
  that the parameter name does not need to be the same name as the variable we
  are passing it.
- At this point in time, `numberCopy` has the same value as `number`, which is
  still 10.
- We will reassign `numberCopy` to the expression `numberCopy + 2`. This
  expression will return the value of 12.
- We will print that the `numberCopy` value is now 12 and exit the method
  `change()` since there are no more statements left to execute.
- The code has now returned to the `main()` method and prints out the value
  of `number` - which is still 10 since the method `change()` was only making
  adjustments to the copy of `number` that we sent it.

```plainttext
number before method call is 10
numberCopy in the method call is 12
number after method call is 10
```

## References Pass-By-Value

Cool - we now understand how pass-by-value works! But we have only seen how it
works with a primitive type, like `int`. What if we want to pass a reference
type? Can we still do that if Java only allows us to pass-by-value?

The answer is yes! But Java will still treat it as pass-by-value. Let's look at
an example and how this might work in memory:

```java
public class Example {
  private int number;

  public Example(int number) {
    this.number = number;
  }

  public int getNumber() {
    return number;
  }

  public void setNumber(int number) {
    this.number = number;
  }
  
  public static void main(String[] args) {
      Example mainExample = new Example(10);
      System.out.println("number before method call is " + mainExample.getNumber());
      change(mainExample);
      System.out.println("number after method call is " + mainExample.getNumber());
  }

  public static void change(Example exampleCopy) {
      exampleCopy.setNumber(exampleCopy.getNumber() + 2);
      System.out.println("number in the method call is " + exampleCopy.getNumber());
  }
}
```

In the code above, notice how we made some changes from our previous example.
We built up the `Example` class to contain a private instance variable `number`
with a constructor, getter, and a setter. Now let's see what happens when we
pass the object of type `Example` to the `change()` method:

- When the code is first executed, we will enter the `main()` method and
  create an object called `mainExample` of type `Example`. In doing so, we
  set the `number` to 10.
- We will then output the value of `number` before calling the `change()`
  method, just as we did before.
- In the `change()` method, we are passing it `mainExample`. Since Java is
  pass-by-value only, we will pass it a copy of `mainExample`. Well remember
  that `mainExample` is a reference type. This means, in memory, it points to
  an object in the heap. So when we copy the reference, `mainExample`, we are
  copying the pointer too. So our copy of the reference will actually point
  to the same `Example` object in the heap!

![Pass reference by value](https://curriculum-content.s3.amazonaws.com/java-mod-1/passing-by-value-vs-reference/References-Same-Object.png)

- The code now enters in the `change()` method and receives a copy of
  `mainExample`. The receiving method chooses to call the copy of `mainExample`
  `exampleCopy`.
- Since the `exampleCopy` reference points to the same object in memory as the
  original `mainCopy`, the value of `number` is still 10.
- We will set `number` to the expression `exampleCopy.getNumber() + 2`. This
  expression will return the value of 12.

![Change Value](https://curriculum-content.s3.amazonaws.com/java-mod-1/passing-by-value-vs-reference/References-Same-Object-Changed.png)

- We will print that the `number` value is now 12 and exit the method
  `change()` since there are no more statements left to execute.
- The code has now returned to the `main()` method and prints out the value
  of `number` - which is now 12 since the copy, `exampleCopy`, was pointing to
  the same object as `mainExample`.

```plainttext
number before method call is 10
number in the method call is 12
number after method call is 12
```

In summary, when we copy a reference that is passed to a method, we are copying
the pointer to the object as well.
