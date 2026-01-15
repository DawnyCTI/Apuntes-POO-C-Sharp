# Propiedades y Constructores

## Propiedades

Las **propiedades** proporcionan un mecanismo flexible para leer, escribir o calcular el valor de un campo privado. Funcionan como campos públicos pero son métodos especiales llamados accessors.

### Sintaxis de Propiedades

```csharp
public class Persona
{
    // Campo privado
    private string nombre;
    
    // Propiedad con get y set
    public string Nombre
    {
        get { return nombre; }
        set { nombre = value; }
    }
}
```

### Propiedades Auto-implementadas

```csharp
public class Producto
{
    // Propiedad auto-implementada
    public string Nombre { get; set; }
    public decimal Precio { get; set; }
    
    // Propiedad con valor por defecto
    public int Stock { get; set; } = 0;
    
    // Propiedad de solo lectura
    public string Codigo { get; }
    
    // Propiedad con setter privado
    public DateTime FechaCreacion { get; private set; }
    
    public Producto(string codigo)
    {
        if (string.IsNullOrWhiteSpace(codigo))
        {
            throw new ArgumentException("El código no puede estar vacío");
        }
        Codigo = codigo;
        FechaCreacion = DateTime.Now;
    }
}
```

### Propiedades con Validación

```csharp
public class CuentaBancaria
{
    private decimal saldo;
    
    public decimal Saldo
    {
        get { return saldo; }
        private set
        {
            if (value < 0)
            {
                throw new ArgumentException("El saldo no puede ser negativo");
            }
            saldo = value;
        }
    }
    
    public void Depositar(decimal cantidad)
    {
        if (cantidad > 0)
        {
            Saldo += cantidad;
        }
    }
}
```

### Propiedades Calculadas

```csharp
public class Rectangulo
{
    public double Ancho { get; set; }
    public double Alto { get; set; }
    
    // Propiedad calculada (solo get)
    public double Area
    {
        get { return Ancho * Alto; }
    }
    
    // Expresión bodied (C# 6.0+)
    public double Perimetro => 2 * (Ancho + Alto);
}
```

### Propiedades con Expression Body

```csharp
public class Persona
{
    public string Nombre { get; set; }
    public string Apellido { get; set; }
    
    // Propiedad de solo lectura con expression body
    public string NombreCompleto => $"{Nombre} {Apellido}";
    
    // Con get y set
    private string email;
    public string Email
    {
        get => email;
        set => email = value.ToLower();
    }
}
```

## Constructores

Los **constructores** son métodos especiales que se ejecutan al crear una instancia de una clase. Inicializan el objeto.

### Constructor Predeterminado

```csharp
public class Persona
{
    public string Nombre { get; set; }
    public int Edad { get; set; }
    
    // Constructor sin parámetros
    public Persona()
    {
        Nombre = "Sin nombre";
        Edad = 0;
    }
}
```

### Constructores Parametrizados

```csharp
public class Persona
{
    public string Nombre { get; set; }
    public int Edad { get; set; }
    
    // Constructor con parámetros
    public Persona(string nombre, int edad)
    {
        Nombre = nombre;
        Edad = edad;
    }
}

// Uso
Persona persona = new Persona("Juan", 25);
```

### Sobrecarga de Constructores

```csharp
public class Empleado
{
    public string Nombre { get; set; }
    public string Puesto { get; set; }
    public decimal Salario { get; set; }
    
    // Constructor 1: Solo nombre
    public Empleado(string nombre)
    {
        Nombre = nombre;
        Puesto = "Por asignar";
        Salario = 0;
    }
    
    // Constructor 2: Nombre y puesto
    public Empleado(string nombre, string puesto)
    {
        Nombre = nombre;
        Puesto = puesto;
        Salario = 0;
    }
    
    // Constructor 3: Todos los parámetros
    public Empleado(string nombre, string puesto, decimal salario)
    {
        Nombre = nombre;
        Puesto = puesto;
        Salario = salario;
    }
}
```

### Encadenamiento de Constructores (this)

```csharp
public class Empleado
{
    public string Nombre { get; set; }
    public string Puesto { get; set; }
    public decimal Salario { get; set; }
    
    // Constructor principal
    public Empleado(string nombre, string puesto, decimal salario)
    {
        Nombre = nombre;
        Puesto = puesto;
        Salario = salario;
    }
    
    // Llama al constructor principal con valores por defecto
    public Empleado(string nombre) : this(nombre, "Por asignar", 0)
    {
    }
    
    public Empleado(string nombre, string puesto) : this(nombre, puesto, 0)
    {
    }
}
```

### Constructor Estático

```csharp
public class Configuracion
{
    public static string ServidorBD { get; private set; }
    public static int Puerto { get; private set; }
    
    // Constructor estático: se ejecuta una sola vez
    static Configuracion()
    {
        ServidorBD = "localhost";
        Puerto = 5432;
        Console.WriteLine("Configuración inicializada");
    }
}
```

### Constructor Privado (Singleton)

```csharp
public class Singleton
{
    private static Singleton instancia;
    
    // Constructor privado
    private Singleton()
    {
        Console.WriteLine("Instancia creada");
    }
    
    public static Singleton ObtenerInstancia()
    {
        if (instancia == null)
        {
            instancia = new Singleton();
        }
        return instancia;
    }
}

// Uso
Singleton s1 = Singleton.ObtenerInstancia();
Singleton s2 = Singleton.ObtenerInstancia();
// s1 y s2 son la misma instancia
```

## Ejemplo Completo: Sistema de Productos

```csharp
using System;
using System.Collections.Generic;

public class Producto
{
    // Campos privados
    private string codigo;
    private decimal precio;
    private int stock;
    
    // Propiedades públicas con validación
    public string Codigo
    {
        get { return codigo; }
        private set
        {
            if (string.IsNullOrWhiteSpace(value))
            {
                throw new ArgumentException("El código no puede estar vacío");
            }
            codigo = value;
        }
    }
    
    public string Nombre { get; set; }
    
    public decimal Precio
    {
        get { return precio; }
        set
        {
            if (value < 0)
            {
                throw new ArgumentException("El precio no puede ser negativo");
            }
            precio = value;
        }
    }
    
    public int Stock
    {
        get { return stock; }
        private set
        {
            if (value < 0)
            {
                throw new ArgumentException("El stock no puede ser negativo");
            }
            stock = value;
        }
    }
    
    // Propiedades calculadas
    public decimal PrecioConIVA => Precio * 1.21m;
    public bool DisponibleEnStock => Stock > 0;
    public string Estado => Stock > 10 ? "En stock" : Stock > 0 ? "Pocas unidades" : "Agotado";
    
    // Propiedad de solo lectura
    public DateTime FechaCreacion { get; }
    
    // Constructor principal
    public Producto(string codigo, string nombre, decimal precio, int stock)
    {
        Codigo = codigo;
        Nombre = nombre;
        Precio = precio;
        Stock = stock;
        FechaCreacion = DateTime.Now;
    }
    
    // Constructor con valores por defecto
    public Producto(string codigo, string nombre, decimal precio) 
        : this(codigo, nombre, precio, 0)
    {
    }
    
    // Métodos para gestionar stock
    public void AgregarStock(int cantidad)
    {
        if (cantidad > 0)
        {
            Stock += cantidad;
            Console.WriteLine($"Stock actualizado: {Stock} unidades");
        }
    }
    
    public bool VenderProducto(int cantidad)
    {
        if (cantidad > 0 && cantidad <= Stock)
        {
            Stock -= cantidad;
            Console.WriteLine($"Venta realizada. Stock restante: {Stock}");
            return true;
        }
        Console.WriteLine("No hay suficiente stock");
        return false;
    }
    
    public void MostrarInformacion()
    {
        Console.WriteLine($"Código: {Codigo}");
        Console.WriteLine($"Nombre: {Nombre}");
        Console.WriteLine($"Precio: {Precio:C}");
        Console.WriteLine($"Precio con IVA: {PrecioConIVA:C}");
        Console.WriteLine($"Stock: {Stock} ({Estado})");
        Console.WriteLine($"Fecha de creación: {FechaCreacion:dd/MM/yyyy HH:mm}");
    }
}

// Producto con descuento
public class ProductoConDescuento : Producto
{
    public decimal PorcentajeDescuento { get; set; }
    
    public decimal PrecioConDescuento => Precio * (1 - PorcentajeDescuento / 100);
    
    public decimal PrecioFinalConIVA => PrecioConDescuento * 1.21m;
    
    public ProductoConDescuento(string codigo, string nombre, decimal precio, 
                                 int stock, decimal descuento)
        : base(codigo, nombre, precio, stock)
    {
        if (descuento < 0 || descuento > 100)
        {
            throw new ArgumentException("El descuento debe estar entre 0 y 100");
        }
        PorcentajeDescuento = descuento;
    }
    
    public new void MostrarInformacion()
    {
        base.MostrarInformacion();
        Console.WriteLine($"Descuento: {PorcentajeDescuento}%");
        Console.WriteLine($"Precio con descuento: {PrecioConDescuento:C}");
        Console.WriteLine($"Precio final (con IVA): {PrecioFinalConIVA:C}");
    }
}

class Program
{
    static void Main(string[] args)
    {
        // Crear productos
        Producto producto1 = new Producto("P001", "Laptop", 999.99m, 15);
        ProductoConDescuento producto2 = new ProductoConDescuento(
            "P002", "Mouse Gaming", 49.99m, 50, 20);
        
        Console.WriteLine("=== PRODUCTO NORMAL ===");
        producto1.MostrarInformacion();
        producto1.VenderProducto(3);
        
        Console.WriteLine("\n=== PRODUCTO CON DESCUENTO ===");
        producto2.MostrarInformacion();
        producto2.AgregarStock(10);
    }
}
```

## Inicializadores de Objetos

```csharp
// En lugar de usar constructores con muchos parámetros
Producto producto = new Producto
{
    Codigo = "P003",
    Nombre = "Teclado",
    Precio = 79.99m,
    Stock = 25
};
```

## Propiedades Requeridas (C# 11+)

```csharp
public class Usuario
{
    public required string Nombre { get; set; }
    public required string Email { get; set; }
    public string Telefono { get; set; } // Opcional
}

// Uso: El compilador obliga a inicializar las propiedades requeridas
Usuario usuario = new Usuario
{
    Nombre = "Juan",
    Email = "juan@example.com"
    // Telefono es opcional
};
```

## Mejores Prácticas

### Propiedades:
- Usa propiedades en lugar de campos públicos
- Valida los datos en los setters
- Usa propiedades auto-implementadas para casos simples
- Considera propiedades de solo lectura para valores inmutables
- Usa expression-bodied members para propiedades calculadas simples

### Constructores:
- Siempre valida los parámetros del constructor
- Usa encadenamiento de constructores para evitar duplicación
- Inicializa todos los campos necesarios en el constructor
- Considera usar inicializadores de objetos para flexibilidad
- Usa constructores privados para implementar patrones como Singleton
