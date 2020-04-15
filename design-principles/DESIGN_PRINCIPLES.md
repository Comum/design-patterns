# Design Principles

Here is a small list of universal principles of software design that describe some good practices that can and should be followed.

### Encapsulate What Varies

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

### Program to an Interface, not an Implementation

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
