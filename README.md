# Desgin Patterns

![](0sHtYG-Km7Dh80EBj.webp)

## Table of Contents
1. [Overview](#Overview)
2. [Creational Design Patterns](#Creational-Design-Patterns)
3. [Structural Design Patterns](#Structural-Design-Patterns)
4. [Behavioral Design Patterns](#Behavioral-Design-Patterns)
5. [Conclusion](#Conclusion)

 ---

## Overview

Design patterns are essential tools in software development that provide proven solutions to common design problems.
In C#, developers can leverage a variety of design patterns to build robust and maintainable software.
In this article, we will explore some of the most common design patterns in C# and provide real-world examples for each category.

## Creational Design Patterns

Creational design patterns deal with object creation mechanisms. They help in controlling the object instantiation process and provide flexibility in how objects are created.

# Factory Pattern
Problem: Creating objects directly can lead to tight coupling between client code and concrete classes, making it hard to adapt to changes or extend the system with new classes.

The Factory Pattern solves this by providing an interface to create objects without exposing their concrete implementations.

Solution: The Factory Pattern defines a factory class or method responsible for creating objects. Clients request objects from the factory using a common interface, allowing them to work with abstract types while leaving the concrete object creation to the factory.

```C#
// Example: Creating different types of vehicles using a factory
public interface IVehicle
{
    void Drive();
}

public class Car : IVehicle
{
    public void Drive() => Console.WriteLine("Driving a car");
}

public class Bicycle : IVehicle
{
    public void Drive() => Console.WriteLine("Riding a bicycle");
}

public class VehicleFactory
{
    public IVehicle CreateVehicle(string type)
    {
        switch (type)
        {
            case "car": return new Car();
            case "bicycle": return new Bicycle();
            default: throw new ArgumentException("Invalid vehicle type");
        }
    }
}
```
# Builder Pattern
Problem: When dealing with complex objects with many configuration options, constructors with numerous parameters become unwieldy and error-prone.

The Builder Pattern simplifies object creation by separating the construction process from the actual representation.

Solution: The Builder Pattern introduces a director class that orchestrates the construction of complex objects using a builder interface. The builder interface provides methods to set the object’s properties step by step, resulting in a more readable and maintainable way to create objects with various configurations.

```C#
// Product class
class Pizza
{
    public string Dough { get; set; }
    public string Sauce { get; set; }
}

// Builder interface
interface IPizzaBuilder
{
    IPizzaBuilder SetDough(string dough);
    IPizzaBuilder SetSauce(string sauce);
    Pizza Build();
}

// Concrete builder
class PizzaBuilder : IPizzaBuilder
{
    private Pizza pizza = new Pizza();

    public IPizzaBuilder SetDough(string dough)
    {
        pizza.Dough = dough;
        return this;
    }

    public IPizzaBuilder SetSauce(string sauce)
    {
        pizza.Sauce = sauce;
        return this;
    }

    public Pizza Build()
    {
        return pizza;
    }
}

// Director (optional)
class PizzaDirector
{
    private readonly IPizzaBuilder pizzaBuilder;

    public PizzaDirector(IPizzaBuilder builder)
    {
        pizzaBuilder = builder;
    }

    public Pizza CreateVegetarianPizza()
    {
        return pizzaBuilder
            .SetDough("Whole Wheat")
            .SetSauce("Tomato")
            .Build();
    }

    public Pizza CreatePepperoniPizza()
    {
        return pizzaBuilder
            .SetDough("Thin Crust")
            .SetSauce("Spicy Tomato")
            .Build();
    }
}
```

# Singleton Pattern
Problem: Ensuring that a class has only one instance can be challenging, especially when multiple parts of the codebase need access to that instance.

The Singleton Pattern addresses this by restricting the instantiation of a class to a single instance and providing a global point of access to that instance.

Solution: The Singleton Pattern involves creating a private constructor to prevent direct instantiation and a static method or property to access the single instance. The first time the instance is requested, it’s created; subsequent requests return the existing instance, ensuring that there’s only one instance of the class.

```C#
// Example: Creating a single configuration manager
public class ConfigurationManager
{
    private static ConfigurationManager _instance;

    private ConfigurationManager() { }

    public static ConfigurationManager Instance
    {
        get
        {
            if (_instance == null)
            {
                _instance = new ConfigurationManager();
            }
            return _instance;
        }
    }

    public string GetSetting(string key)
    {
        // Retrieve setting from configuration file
        return "Value for " + key;
    }
}
```

## Structural Design Patterns

Structural Design Patterns are a set of design principles used in software engineering to solve common problems related to object composition, simplify code, and enhance flexibility and scalability. They focus on the relationships between objects and how they can be structured to achieve better code organization.

# Adapter Pattern
Problem: Integrating new components or libraries with incompatible interfaces into an existing system can be problematic.

The Adapter Pattern allows these components to work together by creating an adapter that acts as a bridge between the incompatible interfaces.

Solution: The Adapter Pattern defines an adapter class that implements the expected interface for the client code. This adapter class delegates calls to the methods of the adapted object, enabling it to interact with the client code without exposing the incompatible interface.

```C#
// Example: Adapting an old printer to a modern interface
public interface IMachine
{
    void Start();
    void Stop();
}

public class OldPrinter
{
    public void PowerOn() => Console.WriteLine("Old printer powered on");
    public void PowerOff() => Console.WriteLine("Old printer powered off");
}

public class PrinterAdapter : IMachine
{
    private readonly OldPrinter _printer;

    public PrinterAdapter(OldPrinter printer)
    {
        _printer = printer;
    }

    public void Start() => _printer.PowerOn();
    public void Stop() => _printer.PowerOff();
}
```

# Bridge Pattern
Problem: Separating an abstraction from its implementation is crucial in large software systems to promote flexibility and maintainability.

The Bridge Pattern achieves this separation by defining two separate hierarchies for abstraction and implementation, allowing them to evolve independently.

Solution: The Bridge Pattern involves creating an abstract class (abstraction) that contains a reference to an interface (implementation). The abstraction delegates the implementation details to the interface, enabling different implementations to be used interchangeably without affecting the abstraction.

```C#
// Example: Implementing different shapes with various drawing methods
public interface IDrawAPI
{
    void DrawCircle(int radius);
}

public class RedCircle : IDrawAPI
{
    public void DrawCircle(int radius) => Console.WriteLine($"Drawing Red Circle of radius {radius}");
}

public class GreenCircle : IDrawAPI
{
    public void DrawCircle(int radius) => Console.WriteLine($"Drawing Green Circle of radius {radius}");
}

public abstract class Shape
{
    protected IDrawAPI drawAPI;

    protected Shape(IDrawAPI drawAPI)
    {
        this.drawAPI = drawAPI;
    }

    public abstract void Draw();
}

public class Circle : Shape
{
    private int radius;

    public Circle(int radius, IDrawAPI drawAPI) : base(drawAPI)
    {
        this.radius = radius;
    }

    public override void Draw() => drawAPI.DrawCircle(radius);
}
```

# Decorator Pattern
Problem: Adding new responsibilities to objects dynamically without modifying their class hierarchy can be challenging.

The Decorator Pattern addresses this by attaching additional behaviors to objects without altering their structure.

Solution: The Decorator Pattern introduces decorator classes that wrap the core object (component) and provide additional functionality. These decorators implement the same interface as the core object, allowing them to be stacked and combined to extend the object’s behavior while keeping the core object intact.

```C#
// Example: Adding toppings to a pizza
public abstract class Pizza
{
    public abstract string GetDescription();
    public abstract double GetCost();
}

public class MargheritaPizza : Pizza
{
    public override string GetDescription() => "Margherita Pizza";
    public override double GetCost() => 6.99;
}

public abstract class PizzaDecorator : Pizza
{
    protected Pizza pizza;

    public PizzaDecorator(Pizza pizza)
    {
        this.pizza = pizza;
    }

    public override string GetDescription() => pizza.GetDescription();
    public override double GetCost() => pizza.GetCost();
}

public class ExtraCheese : PizzaDecorator
{
    public ExtraCheese(Pizza pizza) : base(pizza) { }

    public override string GetDescription() => $"{pizza.GetDescription()}, Extra Cheese";
    public override double GetCost() => pizza.GetCost() + 1.50;
}
```

## Behavioral Design Patterns

Behavioral Design Patterns in software engineering focus on how objects interact and communicate with each other. They deal with the responsibility distribution among objects and describe patterns for managing algorithms, responsibilities, and communication between objects.

# Observer Pattern
Problem: Establishing dependencies between objects that need to be notified of changes in another object’s state can lead to tight coupling.

The Observer Pattern solves this by defining a one-to-many relationship, where the subject notifies multiple observers of changes without knowing their specific types.

Solution: The Observer Pattern involves a subject (observable) that maintains a list of observers. When the subject’s state changes, it notifies all registered observers. Observers implement an interface defining an update method, enabling them to respond to changes in the subject’s state.

```C#
// Example: Implementing a stock market observer
public interface IObserver
{
    void Update(string message);
}

public class Stock : IObserver
{
    private string symbol;
    private double price;

    public Stock(string symbol, double price)
    {
        this.symbol = symbol;
        this.price = price;
    }

    public void Update(string message)
    {
        Console.WriteLine($"{symbol} - Price: {price} - {message}");
    }
}

public class StockMarket
{
    private List<IObserver> observers = new List<IObserver>();

    public void AddObserver(IObserver observer)
    {
        observers.Add(observer);
    }

    public void UpdatePrices()
    {
        // Simulate price changes and notify observers
        foreach (var observer in observers)
        {
            observer.Update("Price increased by 0.5%");
        }
    }
}
```

# Command Pattern
Problem: Encapsulating requests as objects allows for flexibility in command handling, such as queuing, logging, or undoing operations.

The Command Pattern achieves this by encapsulating a request as a command object, separating the sender from the receiver.

Solution: The Command Pattern defines command objects that encapsulate specific actions and parameters. These commands implement a common interface with an execute method. An invoker class invokes the command, which can be easily extended or swapped without altering the sender-receiver relationship.

```C#
// Example: Implementing a remote control with different commands
public interface ICommand
{
    void Execute();
}

public class Light
{
    public void TurnOn() => Console.WriteLine("Light is ON");
    public void TurnOff() => Console.WriteLine("Light is OFF");
}

public class LightOnCommand : ICommand
{
    private Light light;

    public LightOnCommand(Light light)
    {
        this.light = light;
    }

    public void Execute() => light.TurnOn();
}

public class LightOffCommand : ICommand
{
    private Light light;

    public LightOffCommand(Light light)
    {
        this.light = light;
    }

    public void Execute() => light.TurnOff();
}
```

# Strategy Pattern
Problem: Switching between different algorithms or behaviors at runtime can be challenging without introducing conditional statements.

The Strategy Pattern provides a solution by defining a family of interchangeable algorithms and making them easy to switch.

Solution: The Strategy Pattern involves defining a set of algorithms as separate classes, each implementing a common interface. The context class holds a reference to a strategy object and delegates the algorithm’s execution to it. This allows you to change strategies at runtime, promoting flexibility and maintainability.

```C#
// Example: Sorting a list using different sorting algorithms
public interface ISortStrategy
{
    void Sort(List<int> list);
}

public class BubbleSort : ISortStrategy
{
    public void Sort(List<int> list)
    {
        Console.WriteLine("Sorting using Bubble Sort");
        // Implement Bubble Sort algorithm
    }
}

public class QuickSort : ISortStrategy
{
    public void Sort(List<int> list)
    {
        Console.WriteLine("Sorting using Quick Sort");
        // Implement Quick Sort algorithm
    }
}

public class SortContext
{
    private ISortStrategy strategy;

    public SortContext(ISortStrategy strategy)
    {
        this.strategy = strategy;
    }

    public void SortList(List<int> list)
    {
        strategy.Sort(list);
    }
}
```

## Conclusion
Design patterns are a powerful tool for C# developers to improve the design and maintainability of their software. By understanding and applying these patterns, you can write more efficient, flexible, and maintainable code.

Whether you’re solving object creation, composition, or interaction problems, design patterns offer proven solutions to common challenges in software development. Start incorporating these patterns into your C# projects to build cleaner and more maintainable code.
