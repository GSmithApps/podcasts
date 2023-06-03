pattern matching and reflection uses runtime type,
which in a way, is a runtime value, but still in
all those cases (except julia?), they still are required
to return the same type

**can we do the runtime polymorphism and overloading
in an immutable language the doesn't allow rereferencing
or changing datatypes of variables? For example, one of the
good things about closing these things off in classes is that
you can trust that the methods aren't getting modified anywhere,
but in julia those could get modified. I'm thinking that maybe
it would be good to allow them to be defined elsewhere, but
not let them be modified after they're defined... i.e. defined once
but not modified after that... i guess
still the benefit ofOO is that you have an expectation of where
in the code those methods will be defined, whereas in julia they could be anywhere**

type stability in julia... different methods of same
function but with same return type

Why do dynamic typing and dynamic dispatch have to go together? Do they not? Are they orthogonal?


**previews of next episode and recap**

# Episode 1

- example of overloading vs overriding
  - two classes: car and civic
  - two functions: one overloaded, and one overridden
  - make a list with one car and one civic (the civic has to be
    a car to typecheck on the list)
  - loop through the list and call ride... prints car ride and civic ride
  - loop through the list and call load... prints car load and car load

i'll go slow
| class | load       | ride       |
| ----- | ---------- | ---------- |
| car   | car load   | car ride   |
| civic | civic load | civic ride |

# Episode 2

- vocabulary
  - function overloading / ad hoc polymorphism / compile time dispatch / early binding
  - runtime polymorphism / dynamic dispatch / method overriding / late binding
- compile time vs runtime
  - when are they different? confusing
    - inheritance tree
    - union types
    - inheritance is union


# Episode 3

- when are same and when are different?
  - not union, can use overload
  - union, can't use overload
- given this is the case, if the argument is a union
  (or subtype), is there a way to get the early binding to
  do what we want?
  - yes, you have to use reflection to see the dynamic, runtime
    type (maybe using pattern matching), and then use the function you want. 
    - for example:
      - in our car example, in the car overload, we could
        have used reflection and said if it's a civic, call
        the civic load function
      - ```cs
        public static void Load(Civic civic) {
            Console.WriteLine("Civic load");
        }

        public static void Load(Car car) {
            if (car is Civic) {
                Load((Civic)car);
            } else {
                Console.WriteLine("Car load");
            }
        }
        ```

# Episode 4

- why does polymorphism matter?
  - civic vs car example


# Episode 5

- for example:
  - ```
    // static dispatch

    f(x: t1) {}

    f(x: t2) {}

    f(x: union(t1 t2)) {
      If type(x) == t1 then f(x cast t1)
      Elsif type(x) == t2 then f(x cast t2)
    }

    // Dynamic dispatch

    f(x: t1) {}

    f(x: t2) {}
    ```

    **describe how this is related to car example**

# Episode 6

- OO langauges have the good one, but with some caveats
  - it only works on a single argument
  - make the variable on which you're dispatching
    unique syntactically
  - single dispatch ... syntax is different... OO
- multiple dispatch

# Episode 7


- where to define the implementations
  - inside the class vs outside the class
- if it is done in a package, you can't modify it
- car vs civic in a package
- Unreasonable effectiveness of multiple dispatch video reference



```cs
using System;
using System.Collections.Generic;

// set up overriding with the following two
public class Car {
    public virtual void Ride() {
        Console.WriteLine("Car ride");
    }
}

public class Civic : Car {
    public override void Ride() {
        Console.WriteLine("Civic ride");
    }
}

public class Program {

    // set up overloading with the following two
    public static void Load(Car car) {
        Console.WriteLine("Car load");
    }

    public static void Load(Civic civic) {
        Console.WriteLine("Civic load");
    }

    public static void Main(string[] args) {
        List<Car> cars = new List<Car>();
        cars.Add(new Civic());
        cars.Add(new Car());

        Console.WriteLine("Demonstrate method overriding:");
        foreach (Car car in cars) {
            car.Ride();
        }

        Console.WriteLine("\nDemonstrate method overloading:");
        foreach (Car car in cars) {
            Load(car);
        }
    }
}

/*
Output:

Demonstrate method overriding:
Civic ride
Car ride

Demonstrate method overloading:
Car load
Car load
*/
```
