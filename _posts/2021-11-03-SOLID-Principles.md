---
title: SOLID Principles
author: Victor Canseco
layout: post
---
# SOLID  -  Los principios del diseño orientado a objetos

## Introducción 

SOLID es un acrónimo de los primeros cinco principios del diseño orientado a objetos, los cuales fueron definidos por Robert C. Martin.

Estos principios establecen mejores practicas para el desarrollo de software con toda la complejidad que implica mantener y extender conforme el proyecto crezca. Al adoptar estas practicas también se elimina (o minimiza) el código basura, el refactorizar y nos permite utilizar metodologías agiles y/o adaptativas de desarrollo de software.

SOLID significa:

* **S** - Single-responsiblity Principle
* **O** - Open-closed Principle
* **L** - Liskov Substitution Principle
* **I** - Interface Segregation Principle
* **D** - Dependency Inversion Principle

A continuación abordaremos cada uno de estos principios para entender como estos cinco principios pueden ayudarte a ser un mejor desarrollador de software.

## Single-responsiblity Principle

Este principio establece lo siguiente:

> Una Clase debe tener una y solo una razón para cambiar, lo cual significa que una clase debe tener solo un trabajo.

Por ejemplo, consideremos una aplicación que utiliza una colección de figuras (círculos y cuadrados), la cual calcula la suma de el área de todas las figuras de la colección.

Primero creamos las clases de las figuras, su constructor y sus propiedades para inicializarlas.

Para los cuadrados necesitamos la longitud de uno de sus lados: _Length_

``` cs
internal class Square
{
    public int Length { get; set; }
    public Square(int _length)
    {
    	Length = _length;
    }
}
```

Para los círculos necesitamos conocer el: _Radius_

``` csharp
internal class Circle
{
    public int Radius { get; set; }
    public Circle(int radius)
    {
    	Radius = radius;
    }
}
```

Después de crear las clases, continuamos con la lógica para la suma de las áreas de todas las figuras. Para esto recordamos algo de geometría básica.

Para calcular el área de un cuadrado: _a = l * l_

El área de un circulo se calcula de la siguiente forma: _a = π * r^2_

``` csharp
class AreaCalculator
{
    public List<object> Shapes { get; set; }
    public AreaCalculator(List<object> _shapes)
    {
        Shapes = _shapes;
    }
    public double Sum()
    {
        double Area = 0;
        foreach (var shape in Shapes)
        {
            if (shape is Square)
            {
                Area += Math.Pow(((Square)shape).Length, 2);
            }
            else
            {
                Area += Math.PI * Math.Pow(((Circle)shape).Radius, 2);
            }
        }

        return Area;
    }
    public void Print() 
    {
        Console.WriteLine($"La suma de las áreas de las figuras es: {Sum()}");
    }
}
```

Ahora llego la hora de integrar todo y probar

``` csharp
class Program
{
    static void Main(string[] args)
    {

        List<Object> Shapes = new List<object>
        {
            new Circle(2),
            new Square(5),
            new Square(6)
        };

        var Areas = new AreaCalculator(Shapes);

        Areas.Print();
    }


}
```



El problema con el método de impresión es que el controla la lógica de la salida de los datos. 

Consideremos un escenario en el que la salida necesita ser convertida a otro formato como puede ser JSON.

Toda esta lógica podría ser manejada por esta clase; sin embargo esto violaría el principio de Single-Responsibility. Nuestra clase AreaCalculator solo debe de preocuparse por la suma de las áreas de las figuras, no le interesa si el usuario desea usarlas en JSON, HTML u otro formato de salida.

Para manejar esto debemos crear una clase separada la cual manejará la lógica que necesitamos para la salida de los datos para el usuario.

```  cs	
class CalculatorOutputter
{
    public AreaCalculator Calculator { get; set; }
    public CalculatorOutputter(AreaCalculator calculator)
    {
        Calculator = calculator;
    }
    public string ToJson()
    {
        return JsonSerializer.Serialize(Calculator.Sum());
    }
    public string ToHtml()
    {
        return $"La suma de las áreas de las figuras es: {Calculator.Sum()}";
    }

}
```

Integramos todo para utilizar la nueva clase y quedaría de la siguiente forma:

``` csharp
class Program
{
    static void Main(string[] args)
    {
        List<Object> Shapes = new List<object>
        {
            new Circle(2),
            new Square(5),
            new Square(6)
        };

        var Areas = new AreaCalculator(Shapes);
        var Salida = new CalculatorOutputter(Areas);

        Console.WriteLine(Salida.ToJson());
        Console.WriteLine(Salida.ToHtml());

    }
}
```

Con esto la lógica que necesitamos para la salida de datos para el usuario queda implementada en la clase CalculatorOutputter con lo cual satisfacemos el principio de Single-Responsibility.

