# SOLID Principles

## The Single Responsibility Principle

Modules should only have one reason to change. 
Modules should be responsible to one and only one actor.

### BAD Example:

```c#
public class Customer
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string Email { get; set; }

    public void AddCustomerToDatabase()
    {
        // Code to add a customer to the database
    }

    public void SendWelcomeEmail()
    {
        // Code to send a welcome email to the customer
    }
}
```

### GOOD Example:

```c#
public class Customer
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string Email { get; set; }
}

public class CustomerRepository
{
    public void AddCustomer(Customer customer)
    {
        // Code to add a customer to the database
    }
}

public class EmailSender
{
    public void SendWelcomeEmail(Customer customer)
    {
        // Code to send a welcome email to the customer
    }
}
```

## Open/Closed Principle

Modules should be open for extension, but closed for modification.
You should be able to add new functionalities without modifying existing code.

### BAD example

```c#
public enum ShapeType
{
    Circle,
    Square
}

public class Shape
{
    public ShapeType Type { get; set; }
    public double Radius { get; set; }
    public double SideLength { get; set; }
}

public class AreaCalculator
{
    public double CalculateArea(Shape shape)
    {
        switch (shape.Type)
        {
            case ShapeType.Circle:
                return Math.PI * Math.Pow(shape.Radius, 2);
            case ShapeType.Square:
                return Math.Pow(shape.SideLength, 2);
            default:
                throw new InvalidOperationException("Shape not supported");
        }
    }
}

```

### Good Example:

```c#
public interface IShape
{
    double CalculateArea();
}

public class Circle : IShape
{
    public double Radius { get; set; }

    public double CalculateArea()
    {
        return Math.PI * Math.Pow(Radius, 2);
    }
}

public class Square : IShape
{
    public double SideLength { get; set; }

    public double CalculateArea()
    {
        return Math.Pow(SideLength, 2);
    }
}

public class AreaCalculator
{
    public double CalculateArea(IShape shape)
    {
        return shape.CalculateArea();
    }
}

```

## Liskov Substitution

Objects created from a subclass should be able to take the place of objects 
created from the parent class without causing any issues in the program.

### BAD Example:

```c#
public class Rectangle
{
    public virtual int Width { get; set; }
    public virtual int Height { get; set; }

    public int CalculateArea()
    {
        return Width * Height;
    }
}

public class Square : Rectangle
{
    private int _side;

    public override int Width
    {
        get { return _side; }
        set { _side = value; }
    }

    public override int Height
    {
        get { return _side; }
        set { _side = value; }
    }
}

```

### GOOD example:

```c#
public interface IShape
{
    int CalculateArea();
}

public class Rectangle : IShape
{
    public int Width { get; set; }
    public int Height { get; set; }

    public int CalculateArea()
    {
        return Width * Height;
    }
}

public class Square : IShape
{
    public int SideLength { get; set; }

    public int CalculateArea()
    {
        return SideLength * SideLength;
    }
}

```



