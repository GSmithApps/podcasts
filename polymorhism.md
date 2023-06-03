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

- example of overloading vs overriding
  - two classes: car and civic
  - two functions: one overloaded, and one overridden
  - make a list with one car and one civic (the civic has to be
    a car to typecheck on the list)
  - loop through the list and call ride... prints car ride and civic ride
  - loop through the list and call load... prints car load and car load
- compile time vs runtime
  - the union types are determined
    - think of inheritance and subtypes as union types
    - union types are ordered
      - inheritance/subtypes
      - normal union types... union vs individual
- function overloading/ad hoc polymorphism
  - compile time dispatch
- runtime polymorphism / dynamic dispatch / function overriding
  - single dispatch ... syntax is different... OO
    - make the variable on which you're dispatching
      unique syntactically
  - multiple dispatch
- compile time vs runtime (i.e. overloading vs overriding)
  - overloading is done for completely diff types, i.e. 
    not in a hierarchy or inheritance tree (usually... there
    it could also be done inside a hierarchy... more on that
    later), whereas overriding is done in an inheritance tree
- OO langauges have the good one, but with some caveats
  - it only works on a single argument
  - if it is done in a package, you can't modify it
- where to define the implementations
  - inside the class vs outside the class


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
