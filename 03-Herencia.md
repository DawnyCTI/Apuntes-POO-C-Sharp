# Herencia

## ¿Qué es la Herencia?

La **herencia** es un mecanismo que permite crear nuevas clases basadas en clases existentes. La clase nueva (derivada o hija) hereda atributos y métodos de la clase existente (base o padre), permitiendo la reutilización y extensión del código.

## Sintaxis Básica

```csharp
// Clase base
public class Animal
{
    public string Nombre { get; set; }
    
    public void Comer()
    {
        Console.WriteLine($"{Nombre} está comiendo.");
    }
}

// Clase derivada
public class Perro : Animal
{
    public void Ladrar()
    {
        Console.WriteLine($"{Nombre} está ladrando: ¡Guau guau!");
    }
}
```

## Ejemplo de Uso

```csharp
Perro miPerro = new Perro();
miPerro.Nombre = "Max";
miPerro.Comer();   // Método heredado de Animal
miPerro.Ladrar();  // Método propio de Perro
```

## La palabra clave `base`

La palabra clave `base` permite acceder a miembros de la clase base:

```csharp
public class Empleado
{
    public string Nombre { get; set; }
    public decimal Salario { get; set; }
    
    public Empleado(string nombre, decimal salario)
    {
        Nombre = nombre;
        Salario = salario;
    }
    
    public virtual void MostrarInformacion()
    {
        Console.WriteLine($"Empleado: {Nombre}");
        Console.WriteLine($"Salario: {Salario:C}");
    }
}

public class Gerente : Empleado
{
    public string Departamento { get; set; }
    
    public Gerente(string nombre, decimal salario, string departamento)
        : base(nombre, salario) // Llama al constructor de la clase base
    {
        Departamento = departamento;
    }
    
    public override void MostrarInformacion()
    {
        base.MostrarInformacion(); // Llama al método de la clase base
        Console.WriteLine($"Departamento: {Departamento}");
    }
}
```

## Modificadores `virtual` y `override`

- **virtual**: Indica que un método puede ser sobrescrito en clases derivadas
- **override**: Sobrescribe un método virtual de la clase base

```csharp
public class Figura
{
    public virtual double CalcularArea()
    {
        return 0;
    }
}

public class Circulo : Figura
{
    public double Radio { get; set; }
    
    public override double CalcularArea()
    {
        return Math.PI * Radio * Radio;
    }
}

public class Rectangulo : Figura
{
    public double Ancho { get; set; }
    public double Alto { get; set; }
    
    public override double CalcularArea()
    {
        return Ancho * Alto;
    }
}
```

## El modificador `sealed`

`sealed` previene que una clase sea heredada:

```csharp
public sealed class ClaseFinal
{
    // Esta clase no puede ser heredada
}

// Esto causaría un error:
// public class ClaseDerivada : ClaseFinal { }
```

## Ejemplo Completo: Sistema de Vehículos

```csharp
using System;

// Clase base
public class Vehiculo
{
    public string Marca { get; set; }
    public string Modelo { get; set; }
    public int Año { get; set; }
    
    public Vehiculo(string marca, string modelo, int año)
    {
        Marca = marca;
        Modelo = modelo;
        Año = año;
    }
    
    public virtual void Arrancar()
    {
        Console.WriteLine($"{Marca} {Modelo} está arrancando...");
    }
    
    public virtual void Detener()
    {
        Console.WriteLine($"{Marca} {Modelo} se ha detenido.");
    }
    
    public virtual void MostrarInformacion()
    {
        Console.WriteLine($"Vehículo: {Marca} {Modelo} ({Año})");
    }
}

// Clase derivada: Coche
public class Coche : Vehiculo
{
    public int NumeroPuertas { get; set; }
    
    public Coche(string marca, string modelo, int año, int numeroPuertas)
        : base(marca, modelo, año)
    {
        NumeroPuertas = numeroPuertas;
    }
    
    public override void MostrarInformacion()
    {
        base.MostrarInformacion();
        Console.WriteLine($"Número de puertas: {NumeroPuertas}");
    }
    
    public void AbrirMaletero()
    {
        Console.WriteLine("Abriendo maletero...");
    }
}

// Clase derivada: Motocicleta
public class Motocicleta : Vehiculo
{
    public bool TieneSidecar { get; set; }
    
    public Motocicleta(string marca, string modelo, int año, bool tieneSidecar)
        : base(marca, modelo, año)
    {
        TieneSidecar = tieneSidecar;
    }
    
    public override void Arrancar()
    {
        Console.WriteLine($"{Marca} {Modelo} arrancando con un rugido...");
    }
    
    public override void MostrarInformacion()
    {
        base.MostrarInformacion();
        Console.WriteLine($"Tiene sidecar: {(TieneSidecar ? "Sí" : "No")}");
    }
    
    public void HacerCaballito()
    {
        Console.WriteLine("¡Haciendo caballito!");
    }
}

class Program
{
    static void Main(string[] args)
    {
        // Crear objetos de clases derivadas
        Coche coche = new Coche("Toyota", "Corolla", 2022, 4);
        Motocicleta moto = new Motocicleta("Harley-Davidson", "Sportster", 2021, false);
        
        // Usar métodos heredados y propios
        Console.WriteLine("=== COCHE ===");
        coche.MostrarInformacion();
        coche.Arrancar();
        coche.AbrirMaletero();
        coche.Detener();
        
        Console.WriteLine("\n=== MOTOCICLETA ===");
        moto.MostrarInformacion();
        moto.Arrancar();
        moto.HacerCaballito();
        moto.Detener();
    }
}
```

## Tipos de Herencia en C#

C# solo soporta **herencia simple** (una clase solo puede heredar de una clase base), pero puede implementar múltiples interfaces.

```csharp
// Válido: Una clase base
public class ClaseDerivada : ClaseBase { }

// Válido: Una clase base y múltiples interfaces
public class ClaseDerivada : ClaseBase, IInterfaz1, IInterfaz2 { }

// No válido: Múltiples clases base
// public class ClaseDerivada : ClaseBase1, ClaseBase2 { } // ERROR
```

## Ventajas de la Herencia

1. **Reutilización de código**: No es necesario duplicar código común
2. **Jerarquía lógica**: Modela relaciones "es un" del mundo real
3. **Extensibilidad**: Facilita agregar nuevas funcionalidades
4. **Polimorfismo**: Permite tratar objetos de clases derivadas como objetos de la clase base
5. **Mantenimiento**: Los cambios en la clase base se propagan a las derivadas

## Mejores Prácticas

- Usa herencia para relaciones "es un" (un Perro **es un** Animal)
- Prefiere composición sobre herencia cuando sea apropiado
- Diseña las clases base pensando en la extensibilidad
- Usa `virtual` solo cuando esperas que el método sea sobrescrito
- Documenta los contratos esperados en métodos virtuales
