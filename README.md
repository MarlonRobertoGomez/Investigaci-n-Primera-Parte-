using System;

// Singleton Pattern
public sealed class Singleton
{
    private static Singleton? instance;
    private static readonly object lockObject = new object();
    private Singleton() { }
    public static Singleton Instance
    {
        get
        {
            lock (lockObject)
            {
                if (instance == null)
                    instance = new Singleton();
                return instance;
            }
        }
    }
    public void ShowMessage() => Console.WriteLine("Singleton instance called!");
}

// Factory Method Pattern
public abstract class Product
{
    public abstract void Operation();
}

public class ConcreteProductA : Product
{
    public override void Operation() => Console.WriteLine("ConcreteProductA created.");
}

public class ConcreteProductB : Product
{
    public override void Operation() => Console.WriteLine("ConcreteProductB created.");
}

public abstract class Creator
{
    public abstract Product FactoryMethod();
}

public class ConcreteCreatorA : Creator
{
    public override Product FactoryMethod() => new ConcreteProductA();
}

public class ConcreteCreatorB : Creator
{
    public override Product FactoryMethod() => new ConcreteProductB();
}

// Abstract Factory Pattern
public interface IAbstractProductA
{
    void Display();
}

public interface IAbstractProductB
{
    void Display();
}

public class ProductA1 : IAbstractProductA
{
    public void Display() => Console.WriteLine("ProductA1 created.");
}

public class ProductB1 : IAbstractProductB
{
    public void Display() => Console.WriteLine("ProductB1 created.");
}

public interface IAbstractFactory
{
    IAbstractProductA CreateProductA();
    IAbstractProductB CreateProductB();
}

public class ConcreteFactory1 : IAbstractFactory
{
    public IAbstractProductA CreateProductA() => new ProductA1();
    public IAbstractProductB CreateProductB() => new ProductB1();
}

// Builder Pattern
public class ProductBuilder
{
    public string PartA { get; set; } = "";
    public string PartB { get; set; } = "";
    public void Show() => Console.WriteLine($"Product with {PartA} and {PartB}");
}

public interface IBuilder
{
    void BuildPartA();
    void BuildPartB();
    ProductBuilder GetResult();
}

public class ConcreteBuilder : IBuilder
{
    private ProductBuilder product = new ProductBuilder();
    public void BuildPartA() => product.PartA = "Part A built";
    public void BuildPartB() => product.PartB = "Part B built";
    public ProductBuilder GetResult() => product;
}

// Prototype Pattern
public class Prototype : ICloneable
{
    public string Name { get; set; }
    public Prototype(string name) => Name = name;
    public object Clone() => new Prototype(Name);
    public void Show() => Console.WriteLine($"Prototype Name: {Name}");
}

// Testing all patterns
class Program
{
    static void Main()
    {
        // Singleton Test
        Singleton.Instance.ShowMessage();

        // Factory Method Test
        Creator creator = new ConcreteCreatorA();
        Product product = creator.FactoryMethod();
        product.Operation();

        // Abstract Factory Test
        IAbstractFactory factory = new ConcreteFactory1();
        IAbstractProductA productA = factory.CreateProductA();
        IAbstractProductB productB = factory.CreateProductB();
        productA.Display();
        productB.Display();

        // Builder Test
        IBuilder builder = new ConcreteBuilder();
        builder.BuildPartA();
        builder.BuildPartB();
        ProductBuilder builtProduct = builder.GetResult();
        builtProduct.Show();

        // Prototype Test
        Prototype prototype1 = new Prototype("Prototype 1");
        Prototype prototype2 = (Prototype)prototype1.Clone();
        prototype2.Show();
    }
}

