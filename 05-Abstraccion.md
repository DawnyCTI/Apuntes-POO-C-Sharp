# Abstracción

## ¿Qué es la Abstracción?

La **abstracción** es el proceso de ocultar los detalles complejos de implementación y mostrar solo la funcionalidad esencial al usuario. En C#, se implementa mediante clases abstractas y métodos abstractos.

## Clases Abstractas

Una clase abstracta es una clase que **no puede ser instanciada** directamente y está diseñada para ser heredada.

### Características de las Clases Abstractas

- Se declaran con la palabra clave `abstract`
- No se pueden crear instancias directamente
- Pueden contener métodos abstractos (sin implementación)
- Pueden contener métodos concretos (con implementación)
- Pueden tener campos, propiedades y constructores

## Sintaxis Básica

```csharp
// Clase abstracta
public abstract class Figura
{
    // Propiedad concreta
    public string Color { get; set; }
    
    // Constructor
    public Figura(string color)
    {
        Color = color;
    }
    
    // Método abstracto (sin implementación)
    public abstract double CalcularArea();
    
    // Método abstracto
    public abstract double CalcularPerimetro();
    
    // Método concreto (con implementación)
    public void MostrarColor()
    {
        Console.WriteLine($"Color: {Color}");
    }
}
```

## Implementación de Clases Derivadas

```csharp
public class Circulo : Figura
{
    public double Radio { get; set; }
    
    public Circulo(string color, double radio) : base(color)
    {
        Radio = radio;
    }
    
    // Implementación obligatoria del método abstracto
    public override double CalcularArea()
    {
        return Math.PI * Radio * Radio;
    }
    
    public override double CalcularPerimetro()
    {
        return 2 * Math.PI * Radio;
    }
}

public class Rectangulo : Figura
{
    public double Ancho { get; set; }
    public double Alto { get; set; }
    
    public Rectangulo(string color, double ancho, double alto) : base(color)
    {
        Ancho = ancho;
        Alto = alto;
    }
    
    public override double CalcularArea()
    {
        return Ancho * Alto;
    }
    
    public override double CalcularPerimetro()
    {
        return 2 * (Ancho + Alto);
    }
}
```

## Uso de Clases Abstractas

```csharp
// Esto causaría un error:
// Figura fig = new Figura("rojo"); // ERROR: No se puede instanciar

// Correcto: Usar clases derivadas
Figura circulo = new Circulo("azul", 5);
Figura rectangulo = new Rectangulo("rojo", 4, 6);

Console.WriteLine($"Área del círculo: {circulo.CalcularArea():F2}");
Console.WriteLine($"Área del rectángulo: {rectangulo.CalcularArea():F2}");
```

## Diferencias: Clases Abstractas vs Interfaces

| Característica | Clase Abstracta | Interfaz |
|----------------|-----------------|----------|
| Instanciación | No puede instanciarse | No puede instanciarse |
| Métodos | Puede tener abstractos y concretos | Solo abstractos (C# < 8.0) |
| Campos | Puede tener campos | No puede tener campos |
| Constructores | Puede tener constructores | No tiene constructores |
| Herencia múltiple | No soporta | Soporta múltiples interfaces |
| Modificadores de acceso | Todos los modificadores | Todo es público |

## Ejemplo Completo: Sistema de Empleados

```csharp
using System;
using System.Collections.Generic;

// Clase abstracta base
public abstract class Empleado
{
    // Propiedades comunes
    public string Nombre { get; set; }
    public string Apellido { get; set; }
    public int Id { get; set; }
    
    // Constructor
    protected Empleado(int id, string nombre, string apellido)
    {
        Id = id;
        Nombre = nombre;
        Apellido = apellido;
    }
    
    // Método abstracto: cada tipo de empleado calcula su salario diferente
    public abstract decimal CalcularSalario();
    
    // Método abstracto: cada tipo tiene diferentes beneficios
    public abstract string ObtenerBeneficios();
    
    // Método concreto: común para todos los empleados
    public void MostrarInformacion()
    {
        Console.WriteLine($"ID: {Id}");
        Console.WriteLine($"Nombre: {Nombre} {Apellido}");
        Console.WriteLine($"Tipo: {this.GetType().Name}");
    }
    
    // Método virtual: puede ser sobrescrito pero tiene implementación base
    public virtual void MostrarDetalles()
    {
        MostrarInformacion();
        Console.WriteLine($"Salario: {CalcularSalario():C}");
        Console.WriteLine($"Beneficios: {ObtenerBeneficios()}");
    }
}

// Empleado por horas
public class EmpleadoPorHoras : Empleado
{
    public decimal TarifaPorHora { get; set; }
    public int HorasTrabajadas { get; set; }
    
    public EmpleadoPorHoras(int id, string nombre, string apellido, decimal tarifa, int horas)
        : base(id, nombre, apellido)
    {
        TarifaPorHora = tarifa;
        HorasTrabajadas = horas;
    }
    
    public override decimal CalcularSalario()
    {
        decimal salarioBase = TarifaPorHora * HorasTrabajadas;
        // Bonus por horas extra (más de 160 horas al mes)
        if (HorasTrabajadas > 160)
        {
            int horasExtra = HorasTrabajadas - 160;
            salarioBase += horasExtra * TarifaPorHora * 0.5m;
        }
        return salarioBase;
    }
    
    public override string ObtenerBeneficios()
    {
        return "Seguro médico básico";
    }
    
    public override void MostrarDetalles()
    {
        base.MostrarDetalles();
        Console.WriteLine($"Tarifa por hora: {TarifaPorHora:C}");
        Console.WriteLine($"Horas trabajadas: {HorasTrabajadas}");
    }
}

// Empleado asalariado
public class EmpleadoAsalariado : Empleado
{
    public decimal SalarioMensual { get; set; }
    public decimal BonoAnual { get; set; }
    
    public EmpleadoAsalariado(int id, string nombre, string apellido, decimal salario, decimal bono)
        : base(id, nombre, apellido)
    {
        SalarioMensual = salario;
        BonoAnual = bono;
    }
    
    public override decimal CalcularSalario()
    {
        return SalarioMensual + (BonoAnual / 12);
    }
    
    public override string ObtenerBeneficios()
    {
        return "Seguro médico completo, plan dental, plan de pensiones";
    }
    
    public override void MostrarDetalles()
    {
        base.MostrarDetalles();
        Console.WriteLine($"Salario mensual: {SalarioMensual:C}");
        Console.WriteLine($"Bono anual: {BonoAnual:C}");
    }
}

// Empleado por comisión
public class EmpleadoPorComision : Empleado
{
    public decimal SalarioBase { get; set; }
    public decimal VentasTotales { get; set; }
    public decimal PorcentajeComision { get; set; }
    
    public EmpleadoPorComision(int id, string nombre, string apellido, decimal salarioBase, 
                                decimal ventas, decimal porcentaje)
        : base(id, nombre, apellido)
    {
        SalarioBase = salarioBase;
        VentasTotales = ventas;
        PorcentajeComision = porcentaje;
    }
    
    public override decimal CalcularSalario()
    {
        return SalarioBase + (VentasTotales * PorcentajeComision / 100);
    }
    
    public override string ObtenerBeneficios()
    {
        return "Seguro médico, coche de empresa, bonos por objetivos";
    }
    
    public override void MostrarDetalles()
    {
        base.MostrarDetalles();
        Console.WriteLine($"Salario base: {SalarioBase:C}");
        Console.WriteLine($"Ventas totales: {VentasTotales:C}");
        Console.WriteLine($"Comisión: {PorcentajeComision}%");
    }
}

// Clase para gestionar nómina
public class SistemaNomina
{
    private List<Empleado> empleados = new List<Empleado>();
    
    public void AgregarEmpleado(Empleado empleado)
    {
        empleados.Add(empleado);
        Console.WriteLine($"Empleado {empleado.Nombre} agregado al sistema.\n");
    }
    
    public void ProcesarNomina()
    {
        Console.WriteLine("=== PROCESANDO NÓMINA ===\n");
        decimal totalNomina = 0;
        
        foreach (Empleado empleado in empleados)
        {
            empleado.MostrarDetalles();
            totalNomina += empleado.CalcularSalario();
            Console.WriteLine(new string('-', 50));
            Console.WriteLine();
        }
        
        Console.WriteLine($"TOTAL NÓMINA: {totalNomina:C}\n");
    }
}

class Program
{
    static void Main(string[] args)
    {
        SistemaNomina sistema = new SistemaNomina();
        
        // Agregar diferentes tipos de empleados
        sistema.AgregarEmpleado(new EmpleadoPorHoras(1, "Ana", "Martínez", 15.50m, 170));
        sistema.AgregarEmpleado(new EmpleadoAsalariado(2, "Carlos", "García", 3000m, 6000m));
        sistema.AgregarEmpleado(new EmpleadoPorComision(3, "Laura", "Rodríguez", 1500m, 50000m, 5m));
        
        // Procesar nómina usando abstracción
        sistema.ProcesarNomina();
    }
}
```

## Ventajas de la Abstracción

1. **Simplificación**: Oculta la complejidad mostrando solo lo esencial
2. **Reutilización**: Define comportamiento común en la clase base
3. **Flexibilidad**: Permite diferentes implementaciones de la misma interfaz
4. **Mantenibilidad**: Facilita el mantenimiento y la evolución del código
5. **Diseño**: Fuerza un contrato que las clases derivadas deben cumplir

## Mejores Prácticas

- Usa clases abstractas cuando tengas código común que compartir
- Usa interfaces cuando solo necesites definir un contrato
- Define métodos abstractos para comportamientos que varían entre subclases
- Implementa métodos concretos para funcionalidad compartida
- Considera el principio de sustitución de Liskov al diseñar jerarquías
- No abuses de la abstracción; mantén el diseño simple y comprensible
