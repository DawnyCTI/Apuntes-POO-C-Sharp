# Clases y Objetos

## ¿Qué es una Clase?

Una **clase** es una plantilla o molde que define las características (atributos) y comportamientos (métodos) que tendrán los objetos creados a partir de ella.

## ¿Qué es un Objeto?

Un **objeto** es una instancia de una clase. Es una entidad concreta creada a partir del molde que define la clase.

## Sintaxis Básica en C#

```csharp
// Definición de una clase
public class Persona
{
    // Atributos (campos)
    public string Nombre;
    public int Edad;
    
    // Método
    public void Saludar()
    {
        Console.WriteLine($"Hola, soy {Nombre} y tengo {Edad} años.");
    }
}
```

## Creación de Objetos

```csharp
// Crear una instancia de la clase Persona
Persona persona1 = new Persona();
persona1.Nombre = "Juan";
persona1.Edad = 25;
persona1.Saludar(); // Salida: Hola, soy Juan y tengo 25 años.

// Crear otra instancia
Persona persona2 = new Persona();
persona2.Nombre = "María";
persona2.Edad = 30;
persona2.Saludar(); // Salida: Hola, soy María y tengo 30 años.
```

## Ejemplo Completo

```csharp
using System;

public class Coche
{
    // Atributos
    public string Marca;
    public string Modelo;
    public int Año;
    public string Color;
    
    // Métodos
    public void Arrancar()
    {
        Console.WriteLine($"El {Marca} {Modelo} está arrancando...");
    }
    
    public void Detener()
    {
        Console.WriteLine($"El {Marca} {Modelo} se ha detenido.");
    }
    
    public void MostrarInformacion()
    {
        Console.WriteLine($"Marca: {Marca}");
        Console.WriteLine($"Modelo: {Modelo}");
        Console.WriteLine($"Año: {Año}");
        Console.WriteLine($"Color: {Color}");
    }
}

class Program
{
    static void Main(string[] args)
    {
        // Crear objeto
        Coche miCoche = new Coche();
        miCoche.Marca = "Toyota";
        miCoche.Modelo = "Corolla";
        miCoche.Año = 2022;
        miCoche.Color = "Rojo";
        
        // Usar métodos del objeto
        miCoche.MostrarInformacion();
        miCoche.Arrancar();
        miCoche.Detener();
    }
}
```

## Conceptos Clave

- **Clase**: Plantilla que define la estructura y comportamiento
- **Objeto**: Instancia concreta de una clase
- **Atributos**: Variables que almacenan el estado del objeto
- **Métodos**: Funciones que definen el comportamiento del objeto
- **Instanciación**: Proceso de crear un objeto usando `new`

## Ventajas de usar Clases y Objetos

1. **Reutilización de código**: Puedes crear múltiples objetos de la misma clase
2. **Organización**: Agrupa datos y funcionalidad relacionada
3. **Modelado del mundo real**: Representa entidades del mundo real de forma natural
4. **Mantenibilidad**: Facilita la actualización y el mantenimiento del código
