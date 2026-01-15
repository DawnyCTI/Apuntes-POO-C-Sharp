# Polimorfismo

## ¿Qué es el Polimorfismo?

El **polimorfismo** (del griego "muchas formas") es la capacidad de objetos de diferentes clases de responder al mismo mensaje o método de manera diferente. Permite tratar objetos de diferentes tipos de manera uniforme.

## Tipos de Polimorfismo en C#

### 1. Polimorfismo en Tiempo de Compilación (Estático)

Se logra mediante **sobrecarga de métodos** (method overloading).

```csharp
public class Calculadora
{
    // Suma de dos enteros
    public int Sumar(int a, int b)
    {
        return a + b;
    }
    
    // Suma de tres enteros
    public int Sumar(int a, int b, int c)
    {
        return a + b + c;
    }
    
    // Suma de dos decimales
    public double Sumar(double a, double b)
    {
        return a + b;
    }
    
    // Suma de un arreglo de enteros
    public int Sumar(params int[] numeros)
    {
        int suma = 0;
        foreach (int num in numeros)
        {
            suma += num;
        }
        return suma;
    }
}

// Uso
Calculadora calc = new Calculadora();
Console.WriteLine(calc.Sumar(5, 3));           // 8
Console.WriteLine(calc.Sumar(5, 3, 2));        // 10
Console.WriteLine(calc.Sumar(5.5, 3.2));       // 8.7
Console.WriteLine(calc.Sumar(1, 2, 3, 4, 5));  // 15
```

### 2. Polimorfismo en Tiempo de Ejecución (Dinámico)

Se logra mediante **sobrescritura de métodos** (method overriding) usando `virtual` y `override`.

```csharp
public class Animal
{
    public virtual void HacerSonido()
    {
        Console.WriteLine("El animal hace un sonido");
    }
}

public class Perro : Animal
{
    public override void HacerSonido()
    {
        Console.WriteLine("El perro ladra: ¡Guau!");
    }
}

public class Gato : Animal
{
    public override void HacerSonido()
    {
        Console.WriteLine("El gato maúlla: ¡Miau!");
    }
}

public class Vaca : Animal
{
    public override void HacerSonido()
    {
        Console.WriteLine("La vaca muge: ¡Muuu!");
    }
}
```

## Uso de Polimorfismo con Colecciones

```csharp
// Crear un array de tipo Animal que contiene diferentes tipos
Animal[] animales = new Animal[]
{
    new Perro(),
    new Gato(),
    new Vaca(),
    new Perro()
};

// Cada animal responde de manera diferente al mismo método
foreach (Animal animal in animales)
{
    animal.HacerSonido(); // Polimorfismo en acción
}

/* Salida:
El perro ladra: ¡Guau!
El gato maúlla: ¡Miau!
La vaca muge: ¡Muuu!
El perro ladra: ¡Guau!
*/
```

## Ejemplo Completo: Sistema de Pagos

```csharp
using System;
using System.Collections.Generic;

// Clase base
public abstract class MetodoPago
{
    public string Titular { get; set; }
    
    public MetodoPago(string titular)
    {
        Titular = titular;
    }
    
    // Método abstracto que debe ser implementado por las clases derivadas
    public abstract void ProcesarPago(decimal monto);
    
    // Método virtual que puede ser sobrescrito
    public virtual void MostrarDetalles()
    {
        Console.WriteLine($"Titular: {Titular}");
    }
}

// Clase derivada: Tarjeta de Crédito
public class TarjetaCredito : MetodoPago
{
    public string NumeroTarjeta { get; set; }
    public DateTime FechaExpiracion { get; set; }
    
    public TarjetaCredito(string titular, string numeroTarjeta, DateTime fechaExpiracion)
        : base(titular)
    {
        NumeroTarjeta = numeroTarjeta;
        FechaExpiracion = fechaExpiracion;
    }
    
    public override void ProcesarPago(decimal monto)
    {
        Console.WriteLine($"Procesando pago de {monto:C} con tarjeta de crédito");
        Console.WriteLine($"Últimos 4 dígitos: {NumeroTarjeta.Substring(NumeroTarjeta.Length - 4)}");
        Console.WriteLine("Pago aprobado ✓");
    }
    
    public override void MostrarDetalles()
    {
        base.MostrarDetalles();
        Console.WriteLine($"Tarjeta: **** **** **** {NumeroTarjeta.Substring(NumeroTarjeta.Length - 4)}");
        Console.WriteLine($"Expira: {FechaExpiracion:MM/yy}");
    }
}

// Clase derivada: PayPal
public class PayPal : MetodoPago
{
    public string Email { get; set; }
    
    public PayPal(string titular, string email)
        : base(titular)
    {
        Email = email;
    }
    
    public override void ProcesarPago(decimal monto)
    {
        Console.WriteLine($"Procesando pago de {monto:C} a través de PayPal");
        Console.WriteLine($"Cuenta: {Email}");
        Console.WriteLine("Pago completado ✓");
    }
    
    public override void MostrarDetalles()
    {
        base.MostrarDetalles();
        Console.WriteLine($"Cuenta PayPal: {Email}");
    }
}

// Clase derivada: Transferencia Bancaria
public class TransferenciaBancaria : MetodoPago
{
    public string NumeroCuenta { get; set; }
    public string Banco { get; set; }
    
    public TransferenciaBancaria(string titular, string numeroCuenta, string banco)
        : base(titular)
    {
        NumeroCuenta = numeroCuenta;
        Banco = banco;
    }
    
    public override void ProcesarPago(decimal monto)
    {
        Console.WriteLine($"Procesando transferencia de {monto:C}");
        Console.WriteLine($"Banco: {Banco}");
        Console.WriteLine($"Cuenta: {NumeroCuenta}");
        Console.WriteLine("Transferencia exitosa ✓");
    }
    
    public override void MostrarDetalles()
    {
        base.MostrarDetalles();
        Console.WriteLine($"Banco: {Banco}");
        Console.WriteLine($"Cuenta: {NumeroCuenta}");
    }
}

// Clase que procesa pagos polimórficamente
public class ProcesadorPagos
{
    public void RealizarPago(MetodoPago metodoPago, decimal monto)
    {
        Console.WriteLine("\n--- Iniciando Transacción ---");
        metodoPago.MostrarDetalles();
        Console.WriteLine();
        metodoPago.ProcesarPago(monto);
        Console.WriteLine("--- Transacción Finalizada ---\n");
    }
    
    public void ProcesarMultiplesPagos(List<MetodoPago> metodos, decimal monto)
    {
        foreach (var metodo in metodos)
        {
            RealizarPago(metodo, monto);
        }
    }
}

class Program
{
    static void Main(string[] args)
    {
        // Crear diferentes métodos de pago
        var metodoPago1 = new TarjetaCredito("Juan Pérez", "1234567812345678", new DateTime(2025, 12, 31));
        var metodoPago2 = new PayPal("María García", "maria@example.com");
        var metodoPago3 = new TransferenciaBancaria("Carlos López", "ES1234567890", "Banco Nacional");
        
        // Crear lista polimórfica de métodos de pago
        List<MetodoPago> metodosPago = new List<MetodoPago>
        {
            metodoPago1,
            metodoPago2,
            metodoPago3
        };
        
        // Procesar pagos polimórficamente
        ProcesadorPagos procesador = new ProcesadorPagos();
        procesador.ProcesarMultiplesPagos(metodosPago, 100.00m);
        
        // También se puede procesar individualmente
        Console.WriteLine("=== Pago Individual ===");
        procesador.RealizarPago(metodoPago1, 250.50m);
    }
}
```

## Ventajas del Polimorfismo

1. **Flexibilidad**: Permite escribir código más genérico y flexible
2. **Extensibilidad**: Facilita agregar nuevos tipos sin modificar código existente
3. **Mantenibilidad**: Reduce la duplicación de código
4. **Abstracción**: Permite trabajar con interfaces sin preocuparse por la implementación específica
5. **Diseño limpio**: Facilita aplicar principios SOLID (especialmente Open/Closed)

## El Principio Open/Closed

El polimorfismo ayuda a cumplir el principio Open/Closed: "Las clases deben estar abiertas para extensión pero cerradas para modificación".

```csharp
// En lugar de usar switch/if para cada tipo:
// MAL DISEÑO
public void ProcesarPago(string tipo, decimal monto)
{
    if (tipo == "tarjeta")
    {
        // lógica de tarjeta
    }
    else if (tipo == "paypal")
    {
        // lógica de paypal
    }
    // Agregar nuevo tipo requiere modificar este método
}

// Mejor: Usar polimorfismo
// BUEN DISEÑO
public void ProcesarPago(MetodoPago metodo, decimal monto)
{
    metodo.ProcesarPago(monto);
    // Agregar nuevo tipo solo requiere crear nueva clase
}
```

## Mejores Prácticas

- Usa polimorfismo cuando tengas comportamientos similares con implementaciones diferentes
- Prefiere polimorfismo sobre estructuras condicionales complejas
- Diseña interfaces y clases base pensando en la extensibilidad
- Combina polimorfismo con otros principios de POO
- Usa clases abstractas cuando quieras definir un comportamiento base común
