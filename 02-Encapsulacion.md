# Encapsulación

## ¿Qué es la Encapsulación?

La **encapsulación** es el principio de POO que consiste en ocultar los detalles internos de implementación de una clase y exponer solo lo necesario a través de una interfaz pública. Esto se logra mediante modificadores de acceso.

## Modificadores de Acceso en C#

- **public**: Accesible desde cualquier parte
- **private**: Accesible solo dentro de la misma clase
- **protected**: Accesible dentro de la clase y clases derivadas
- **internal**: Accesible dentro del mismo ensamblado
- **protected internal**: Accesible dentro del mismo ensamblado o clases derivadas

## Ejemplo sin Encapsulación

```csharp
public class CuentaBancaria
{
    public decimal Saldo; // Campo público - NO recomendado
    
    public void Depositar(decimal cantidad)
    {
        Saldo += cantidad;
    }
}

// Problema: Se puede modificar directamente
CuentaBancaria cuenta = new CuentaBancaria();
cuenta.Saldo = -1000; // ¡Esto no debería ser posible!
```

## Ejemplo con Encapsulación

```csharp
public class CuentaBancaria
{
    // Campo privado
    private decimal saldo;
    
    // Propiedad pública con validación
    public decimal Saldo
    {
        get { return saldo; }
        private set { saldo = value; } // Solo se puede modificar internamente
    }
    
    public void Depositar(decimal cantidad)
    {
        if (cantidad > 0)
        {
            saldo += cantidad;
            Console.WriteLine($"Depósito exitoso. Nuevo saldo: {saldo:C}");
        }
        else
        {
            Console.WriteLine("La cantidad debe ser positiva.");
        }
    }
    
    public bool Retirar(decimal cantidad)
    {
        if (cantidad > 0 && cantidad <= saldo)
        {
            saldo -= cantidad;
            Console.WriteLine($"Retiro exitoso. Nuevo saldo: {saldo:C}");
            return true;
        }
        else
        {
            Console.WriteLine("Fondos insuficientes o cantidad inválida.");
            return false;
        }
    }
}
```

## Propiedades Auto-implementadas

C# ofrece una sintaxis más corta para propiedades simples:

```csharp
public class Producto
{
    // Propiedad auto-implementada
    public string Nombre { get; set; }
    public decimal Precio { get; set; }
    
    // Propiedad con setter privado
    public DateTime FechaCreacion { get; private set; }
    
    // Propiedad de solo lectura
    public string Codigo { get; }
    
    public Producto(string codigo)
    {
        Codigo = codigo;
        FechaCreacion = DateTime.Now;
    }
}
```

## Ejemplo Completo

```csharp
using System;

public class Empleado
{
    // Campos privados
    private string nombre;
    private decimal salario;
    private int edad;
    
    // Propiedades públicas con validación
    public string Nombre
    {
        get { return nombre; }
        set
        {
            if (!string.IsNullOrWhiteSpace(value))
            {
                nombre = value;
            }
            else
            {
                throw new ArgumentException("El nombre no puede estar vacío");
            }
        }
    }
    
    public decimal Salario
    {
        get { return salario; }
        set
        {
            if (value >= 0)
            {
                salario = value;
            }
            else
            {
                throw new ArgumentException("El salario no puede ser negativo");
            }
        }
    }
    
    public int Edad
    {
        get { return edad; }
        set
        {
            if (value >= 18 && value <= 65)
            {
                edad = value;
            }
            else
            {
                throw new ArgumentException("La edad debe estar entre 18 y 65");
            }
        }
    }
    
    // Método público
    public void AumentarSalario(decimal porcentaje)
    {
        if (porcentaje > 0 && porcentaje <= 100)
        {
            salario += salario * (porcentaje / 100);
            Console.WriteLine($"Salario actualizado: {salario:C}");
        }
    }
    
    public void MostrarInformacion()
    {
        Console.WriteLine($"Empleado: {nombre}");
        Console.WriteLine($"Edad: {edad} años");
        Console.WriteLine($"Salario: {salario:C}");
    }
}

class Program
{
    static void Main(string[] args)
    {
        Empleado emp = new Empleado();
        emp.Nombre = "Carlos García";
        emp.Edad = 30;
        emp.Salario = 50000;
        
        emp.MostrarInformacion();
        emp.AumentarSalario(10); // Aumento del 10%
        
        // emp.salario = -1000; // Error: 'salario' es privado
    }
}
```

## Ventajas de la Encapsulación

1. **Control de acceso**: Controlas cómo se accede y modifica la información
2. **Validación**: Puedes validar los datos antes de asignarlos
3. **Flexibilidad**: Puedes cambiar la implementación interna sin afectar el código externo
4. **Seguridad**: Proteges los datos sensibles de modificaciones no autorizadas
5. **Mantenibilidad**: Facilita el mantenimiento y la evolución del código

## Mejores Prácticas

- Mantén los campos privados
- Usa propiedades para exponer datos
- Valida los datos en los setters
- Proporciona solo los métodos necesarios
- Oculta los detalles de implementación
