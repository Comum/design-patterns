# SOLID Principles

SOLID is a mnemonic for five design principles intended to make software designs more understandable. flexible and maintainable.

These principles, even though their worth is far proved, aren't required in any and all software developement. There are some specific examples that they can do more harm than good, so use them, but think about their application beforehand.

## Single Responsability Principle

As the name says the idea is to make every class responsible for a single change.

However, this is not applicable in all cases, say your program only has a class with around 200 lines, is this scenario, creating a class for each method might be overkill. Just clear your code to make it as readable as possible and you're good to go.

The biggest gains from this principle comes when your software is quite extensive. Thus having a class change a specific part makes the software much easier to follow.

## Open / Closed Principle

_Classes should be open for extension, but closed for modification_

The main idea of this principle is to keep existing code from breaking when you implement new features.

A class is _open_ if you can extend it, produce a subclass and do what you need with it ( add new methods, override existing ones...). Some programming languages allow you to restric further extension of a class with special keywords, such as final. Using this, the class is no longer _open_. At the same time, the class is _closed_ if it's 100% ready to be used by other classes ( its interface is clearly defined and won't be changed in the future ).

If a class is already developed, tested, reviewed, and included is some framework or otherwise used in an app, trying to mess with its code is risky. Instead of changing the the code of the class directly, you can create a subclass and override parts of the original class that you want to behave differently. You'll achieve your goal, but also won't break any existing clients for the original class.

This principle isn't meant to be applied for all changes. For example, when bugfixing, you shouldn't create a subclass, just update the class accordingly. A child class shouldn't be responsible for the parent's issues.

## Liskov Susbtitution Principle

_When extending a class, remember that you should be able to pass objects of the subclass in place of objects of the parent class without breaking the client code._

This means that the subclass should remain compatible with the behaviour of the superclass. When overriding a method, extend the base behaviour rather than replacing it with something else entirely.

The susbtitution principle is a set of checks that help predict whether a subclass remains compatible with the code that was able to work with objects fo the superclass. This concept is critical when developing libraries and frameworks because your classes are going to be used by other people whose code you can't directly access and change.

-   **Parameter types in a method of a subclass should _match_ or be _more abstract_ than parameter types in the method of the superclass.**

    -   Say there's a class with a method that's supposed to feed cats: `feed(Cat c)`. Client code always passes cat objects into this method.
    -   **Good**: Say you created a subclass that overrode the method so that if can feed any animal ( a superclass of cats ): `feed(Animal c)`. Now if you pass an object of this subclass instead of an object of the superclass to the client code, everything would still work fine. The method can feed all animals, so it can still feed any cat passed by the client.
    -   **Bad**: You created another subclass and restricted the feeding method to only accept Bengal cats ( a subclass of cats ): `feed(BengalCat c)`. What will happen to the client code if you link it with an object like this instead of with the original class? Since the method can only feed a specific breed of cats, it won't serve generic cats passed by the client, breaking all related functionality.

-   **The return type in a method of a subclass should _match_ or be a _subtype_ of the return type in the method of the superclass.** As you can see, requirements for a return type are inverse to requirements for parameter types.

    -   Say you have a class with a method `buyCat(): Cat`. The client code expects to receive any cat as a result of executing this method.
    -   **Good**: A subclass overrides the method as follows: `buyCat(): BengalCat`. The client gets a Bengal cat, which is still a cat, so everything is okay.
    -   **Bad**: a subclass overrides the method as follows: `buyCat(): Animal`. Now the client code breaks since it receives an unknown generic animal ( an alligator? a bear? ) that doesn't fit a structure designed for a cat.

    Another anti-example comes from the world of programming languages with dynamic typing: the base method returns a string, but the overriden method returns a number.

-   **A method in a subclass shouldn't throw types of execptions which the base method isn't expected to throw.** In other words, types of exceptions should _match_ or be _subtypes_ of the ones that the base method is already able to throw. This rule comes from the fact that `try-catch` blocks in the client code target specific types of exceptions which the base method is likely to throw. Therefore, an unexpected exception might slip through the defensive lines of the client code and crash the entire application.

    _In most programming languages, especially statically typed ones ( Java, C#, and others ), these rules are built into the language. You won't be able to compile a program that violates these rules._

-   **A subclass shouldn't strengthen pre-conditions.** For example, the base method has a parameter with type `int`. If a subclass overrides this method and requires that the value of an argument passed to the method should be positive ( by throwing an exception if the value is negative ), this strengthens the pre-conditions. The client code, which used to work fine when passing negative numbers into the method, now breaks if it starts working with an object of this subclass.

-   **A subclass shouldn't weaken post-conditions.** Say you have a class with a method that works with a database. A method of the class is supposed to always close all opened databse connections upon returning a value.

    You created a subclass and changed it so that database connections remain open so you can reuse them. But the client might not know anything about your intentions. Because it expects the methods to close all the connections, it may simply terminate the program right after calling the method, polluting a system with ghost database connections.

-   **Invariants of a superclass must be preserved.** This is probably the least formal rule of all. _Invariants_ are conditions in which an object makes sense. For example, invariants of a cat are having four legs, a tail, ability to meow, etc. The confusing part about invariants is that while they can be defined explicitly in the form of interface contracts or a set of assertions within methods, they could also be implied by certain unit tests and expectations of the client code.

    The rule on invariants is the easiest to violate because you might misunderstand or not realize all of the invariants of a complex class. Therefore, the safest way to extend a class is to introduce new fields and methods, and not mess with any existing members of the superclass. Of course, that's not always doable in real life.

-   **A subclass shouldn't change values of private fields of the superclass.** It turns out that some programming languages let you access private members of a class via reflection mechanisms. Other languages ( Python, JavaScript ) don't have any protection for the private members at all.

## Interface Segregation Principle

_Clients shouldn't be forced to depend on methods they do not use._

Try to make your interfaces narrow enough that client classes don't have to implement behaviours they don't need.

According to interface segregation principle, you should break down "fat" interfaces into more granular and specific ones. Clients should implement only those methods that they really need. Otherwise, a change to a "fat" interface would break even clients that don't use the changed methods.

Class inheritance lets a class have just one superclass, but it doesn't limit the number of interfaces that the class can implement at the same time. Hence, there's no need to cram tons of unrelated methods to a single interface. Break it down into several more refined interfaces - you can implement them all in a single class if needed. However, some classes may be fine with implementing just one of them.

As with the other principles, you can go too far with this one. Don't further divide an interface which is already quite specific. Remember that the more interfaces you create, the more complex your code becomes. Keep the balance.

## Dependency Inversion Principle
