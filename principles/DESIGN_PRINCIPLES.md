# Design Principles

Here is a small list of universal principles of software design that describe some good practices that can and should be followed.

## Encapsulate What Varies

The main objective of this principle is to minimize the effect caused by changes.

For example, if we have an application that needs to calculate a monetary value after tax and this tax varies from country to country, we might come up with a function like:

```
function calculateAfterTaxValue(value, countryCode) {
    	let afterTaxValue = 0;
        let tax;

        switch (countryCode) {
            case 'pt':
                tax = 0.2;
                break;
            case 'uk':
                tax = 0.15;
                break;
            default:
                tax = 0.1;
        }

        return afterTaxValue*tax;
}
```

However, each year the tax can change and be updated. So with that in mind we can create an extra function or method that can be easily identified and then updated.

```
function calculateAfterTaxValue(value, countryCode) {
    	let afterTaxValue = 0;
        const tax = getTaxValue(countryCode);

        return afterTaxValue*tax;
}

function getTaxValue(countryCode) {
    switch (countryCode) {
            case 'pt':
                return 0.2;
            case 'uk':
                return 0.15;
                break;
            default:
                return 0.1;
        }
}
```

## Program to an Interface, not an Implementation

Depend on abstractions, not on concrete classes.

The design is flexible enough if you can easily extend it without breaking any existing code.

The idea behind this principle is that you extend an abstract class that supports your existing problem. but it is not attached to a specific response.

For example. Take the following pseudo code:

```
    class Company {
        getEmployees();

        createSoftware() {
            employees = getEmployees();

            foreach(Employee e in employee) {
                e.doWork();
            }
        };
    }

    class GameDevCompany extends Company {
        getEmployees() {
            return [
                new Designer(),
                new Artist()
            ]
        }
    }

    class OutsourcingCompany extends Company {
        getEmployees() {
            return [
                new Programmer(),
                new Tester()
            ]
        }
    }

    class Employee {
        doWork();
    }

    class Designer extends Employee {
        doWork() {
            console.log('Designs layout');
        }
    }

    class Artist extends Employee {
        doWork() {
            console.log('Draws cutscenes');
        }
    }

    class Programmer extends Employee {
        doWork() {
            console.log('Programs software');
        }
    }

    class Tester extends Employee {
        doWork() {
            console.log('Tests software');
        }
    }
```

From the example above we can see that there are two "main" classes. _Company_ and _Employee_. However we do have a relation between them, as _Company_ will have a variable number of _Employee_. However, because we have set up a layer of abstraction between _Company_ and say _Tester_ ( the _Employee_ interface ), we managed to make _Company_ independent from the subsequent _Employee_ sub class _Tester_. This way we can add as many _Employee_ descriptions that we want, that company will always be able to handle them.

## Favor Composition over Inheritance

When talking about OOP, Inheritance is probably the most obvious and easy way of reusing code. However, it comes with some caveats that become apparent after a program has a lot of classes and changing something is really hard. For example:

- **A subclass can't reduce the interface of the superclass.** You have to implement all abstract methods of the parent class even if you won't be using them.

- **When overriding methods you need to make sure that the new behaviour is compatible with the base one.** It's important because objects of the subclass may be passed to any code that expects objects of the superclass and you don't want that code to break.

- **Inheritance breaks encapsulation of the superclasss.** Because the internal details of the parent class become avialable to the subclass. There might be an opposite situation where a programmer makes a superclass aware of some details of subclass for the sake of making further extension easier.

- **Subclasses are tightly coupled to superclasses.** Any change in a superclass may break the functionality of subclasses.

- **Trying to reuse code through inheritance can lead to creating parallel inheritance hierarchies.** Inheritance usually takes place in a single dimension. But whenever there are two or more dimensions, you have to create lots of classs combinations, bloating the class hierarchy to a ridiculous size.

There is an alternative to inheritance called _Composition_. Whereas inheritance represents the "is a" relationship between classes ( a car _is a_ transport ), composition represents the "has a" relationship ( a car _has an_ engine).

### Example

Imagine that you need to create a catalog app for a car manufacturer. The company makes both cars and trucks; they can be either electric or gas; all models have either manual controls or autopilot.

Taking an inheritance apporach, we could easily end up with something like:

```
class Transport {
    (...)
}

class Car extends Transport {
    (...)
}

class Truck extends Transport {
    (...)
}

```

Now we will need to configure each vehicle.

```

class ElectricCar extends Car {
    (...)
}

class CombustionCar extends Car {
    (...)
}

class AutopilotElectricCar  extends ElectricCar {
    (...)
}

class AutopilotCombustionCar  extends CombustionCar {
    (...)
}

```

And then have to repeat the same struture for the Truck. Each additional parameter results in multiplying the number of subclasses. And we end up with a lot of duplicate code between subclasses, as a subclass can't extend two classes at the same time.

If we take a composition approach. Instead of car objects implementing a behaviour on ther own, they can delegate it to other objects. So we can end up with:

```

class Transport {
    private engine;
    private driver;

    (...)
}

class Engine {
    (...)
}

class Driver {
    (...)
}

class CombustionEngine extends Engine {
    (...)
}

class ElectricEngine extends Engine {
    (...)
}

class Robot extends Driver {
    (...)
}

class Human extends Driver {
    (...)
}

```
