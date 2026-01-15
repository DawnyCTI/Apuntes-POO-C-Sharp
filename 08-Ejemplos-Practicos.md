# Ejemplos Prácticos

Este documento contiene ejemplos prácticos que integran todos los conceptos de POO vistos anteriormente.

## Ejemplo 1: Sistema de Biblioteca

```csharp
using System;
using System.Collections.Generic;
using System.Linq;

// Interfaz para elementos prestables
public interface IPrestable
{
    bool Prestar(string usuario);
    bool Devolver();
    bool EstaPrestado { get; }
}

// Clase abstracta base
public abstract class ElementoBiblioteca : IPrestable
{
    public string Id { get; private set; }
    public string Titulo { get; set; }
    public int AñoPublicacion { get; set; }
    public bool EstaPrestado { get; protected set; }
    protected string UsuarioPrestamo { get; set; }
    
    protected ElementoBiblioteca(string id, string titulo, int año)
    {
        Id = id;
        Titulo = titulo;
        AñoPublicacion = año;
        EstaPrestado = false;
    }
    
    public virtual bool Prestar(string usuario)
    {
        if (!EstaPrestado)
        {
            EstaPrestado = true;
            UsuarioPrestamo = usuario;
            Console.WriteLine($"✓ '{Titulo}' prestado a {usuario}");
            return true;
        }
        Console.WriteLine($"✗ '{Titulo}' ya está prestado");
        return false;
    }
    
    public virtual bool Devolver()
    {
        if (EstaPrestado)
        {
            Console.WriteLine($"✓ '{Titulo}' devuelto por {UsuarioPrestamo}");
            EstaPrestado = false;
            UsuarioPrestamo = null;
            return true;
        }
        Console.WriteLine($"✗ '{Titulo}' no está prestado");
        return false;
    }
    
    public abstract void MostrarInformacion();
}

// Clase Libro
public class Libro : ElementoBiblioteca
{
    public string Autor { get; set; }
    public string ISBN { get; set; }
    public int NumeroPaginas { get; set; }
    
    public Libro(string id, string titulo, int año, string autor, string isbn, int paginas)
        : base(id, titulo, año)
    {
        Autor = autor;
        ISBN = isbn;
        NumeroPaginas = paginas;
    }
    
    public override void MostrarInformacion()
    {
        Console.WriteLine($"📚 LIBRO");
        Console.WriteLine($"   ID: {Id}");
        Console.WriteLine($"   Título: {Titulo}");
        Console.WriteLine($"   Autor: {Autor}");
        Console.WriteLine($"   Año: {AñoPublicacion}");
        Console.WriteLine($"   ISBN: {ISBN}");
        Console.WriteLine($"   Páginas: {NumeroPaginas}");
        Console.WriteLine($"   Estado: {(EstaPrestado ? $"Prestado a {UsuarioPrestamo}" : "Disponible")}");
    }
}

// Clase Revista
public class Revista : ElementoBiblioteca
{
    public int NumeroEdicion { get; set; }
    public string Editorial { get; set; }
    
    public Revista(string id, string titulo, int año, int edicion, string editorial)
        : base(id, titulo, año)
    {
        NumeroEdicion = edicion;
        Editorial = editorial;
    }
    
    // Las revistas solo se prestan por 7 días
    public override bool Prestar(string usuario)
    {
        if (base.Prestar(usuario))
        {
            Console.WriteLine("   Recuerda: Las revistas deben devolverse en 7 días");
            return true;
        }
        return false;
    }
    
    public override void MostrarInformacion()
    {
        Console.WriteLine($"📰 REVISTA");
        Console.WriteLine($"   ID: {Id}");
        Console.WriteLine($"   Título: {Titulo}");
        Console.WriteLine($"   Edición: {NumeroEdicion}");
        Console.WriteLine($"   Editorial: {Editorial}");
        Console.WriteLine($"   Año: {AñoPublicacion}");
        Console.WriteLine($"   Estado: {(EstaPrestado ? $"Prestado a {UsuarioPrestamo}" : "Disponible")}");
    }
}

// Clase DVD
public class DVD : ElementoBiblioteca
{
    public string Director { get; set; }
    public int DuracionMinutos { get; set; }
    
    public DVD(string id, string titulo, int año, string director, int duracion)
        : base(id, titulo, año)
    {
        Director = director;
        DuracionMinutos = duracion;
    }
    
    public override void MostrarInformacion()
    {
        Console.WriteLine($"💿 DVD");
        Console.WriteLine($"   ID: {Id}");
        Console.WriteLine($"   Título: {Titulo}");
        Console.WriteLine($"   Director: {Director}");
        Console.WriteLine($"   Año: {AñoPublicacion}");
        Console.WriteLine($"   Duración: {DuracionMinutos} minutos");
        Console.WriteLine($"   Estado: {(EstaPrestado ? $"Prestado a {UsuarioPrestamo}" : "Disponible")}");
    }
}

// Clase Biblioteca
public class Biblioteca
{
    private List<ElementoBiblioteca> catalogo;
    public string Nombre { get; set; }
    
    public Biblioteca(string nombre)
    {
        Nombre = nombre;
        catalogo = new List<ElementoBiblioteca>();
    }
    
    public void AgregarElemento(ElementoBiblioteca elemento)
    {
        catalogo.Add(elemento);
        Console.WriteLine($"Agregado: {elemento.Titulo}");
    }
    
    public void MostrarCatalogo()
    {
        Console.WriteLine($"\n╔══════════════════════════════════╗");
        Console.WriteLine($"║  Catálogo - {Nombre,-19}║");
        Console.WriteLine($"╚══════════════════════════════════╝\n");
        
        foreach (var elemento in catalogo)
        {
            elemento.MostrarInformacion();
            Console.WriteLine();
        }
        
        Console.WriteLine($"Total de elementos: {catalogo.Count}");
    }
    
    public void MostrarDisponibles()
    {
        var disponibles = catalogo.Where(e => !e.EstaPrestado).ToList();
        Console.WriteLine($"\n=== Elementos Disponibles ({disponibles.Count}) ===");
        foreach (var elemento in disponibles)
        {
            Console.WriteLine($"- {elemento.Titulo} (ID: {elemento.Id})");
        }
    }
    
    public ElementoBiblioteca BuscarPorId(string id)
    {
        return catalogo.FirstOrDefault(e => e.Id == id);
    }
}

// Programa principal
class Program
{
    static void Main(string[] args)
    {
        // Crear biblioteca
        Biblioteca biblioteca = new Biblioteca("Biblioteca Central");
        
        // Agregar elementos
        biblioteca.AgregarElemento(new Libro("L001", "Cien años de soledad", 1967, 
            "Gabriel García Márquez", "978-0307474728", 417));
        biblioteca.AgregarElemento(new Libro("L002", "1984", 1949, 
            "George Orwell", "978-0451524935", 328));
        biblioteca.AgregarElemento(new Revista("R001", "National Geographic", 2023, 
            250, "National Geographic Society"));
        biblioteca.AgregarElemento(new DVD("D001", "Inception", 2010, 
            "Christopher Nolan", 148));
        
        // Mostrar catálogo
        biblioteca.MostrarCatalogo();
        
        // Realizar préstamos
        Console.WriteLine("\n=== Realizando Préstamos ===\n");
        var libro = biblioteca.BuscarPorId("L001");
        libro?.Prestar("Juan Pérez");
        
        var revista = biblioteca.BuscarPorId("R001");
        revista?.Prestar("María García");
        
        // Intentar prestar elemento ya prestado
        libro?.Prestar("Carlos López");
        
        // Mostrar disponibles
        biblioteca.MostrarDisponibles();
        
        // Devolver
        Console.WriteLine("\n=== Devoluciones ===\n");
        libro?.Devolver();
        
        // Mostrar disponibles actualizado
        biblioteca.MostrarDisponibles();
    }
}
```

## Ejemplo 2: Sistema de Gestión de Empleados y Departamentos

```csharp
using System;
using System.Collections.Generic;
using System.Linq;

// Interfaz para objetos que pueden generar reportes
public interface IReportable
{
    string GenerarReporte();
}

// Clase Persona base
public abstract class Persona
{
    public string Id { get; protected set; }
    public string Nombre { get; set; }
    public string Apellido { get; set; }
    public DateTime FechaNacimiento { get; set; }
    
    public string NombreCompleto => $"{Nombre} {Apellido}";
    public int Edad => DateTime.Now.Year - FechaNacimiento.Year;
    
    protected Persona(string id, string nombre, string apellido, DateTime fechaNacimiento)
    {
        Id = id;
        Nombre = nombre;
        Apellido = apellido;
        FechaNacimiento = fechaNacimiento;
    }
    
    public abstract void MostrarInformacion();
}

// Clase Empleado
public class Empleado : Persona, IReportable
{
    public string Departamento { get; set; }
    public decimal SalarioBase { get; set; }
    public DateTime FechaContratacion { get; private set; }
    private List<string> proyectos;
    
    public int AñosEnEmpresa => DateTime.Now.Year - FechaContratacion.Year;
    public decimal SalarioConBonos => CalcularSalarioTotal();
    
    public Empleado(string id, string nombre, string apellido, DateTime fechaNacimiento,
                    string departamento, decimal salario, DateTime? fechaContratacion = null)
        : base(id, nombre, apellido, fechaNacimiento)
    {
        Departamento = departamento;
        SalarioBase = salario;
        FechaContratacion = fechaContratacion ?? DateTime.Now;
        proyectos = new List<string>();
    }
    
    private decimal CalcularSalarioTotal()
    {
        decimal bono = 0;
        
        // Bono por antigüedad
        if (AñosEnEmpresa >= 5)
            bono += SalarioBase * 0.10m;
        else if (AñosEnEmpresa >= 3)
            bono += SalarioBase * 0.05m;
        
        // Bono por proyectos
        bono += proyectos.Count * 100;
        
        return SalarioBase + bono;
    }
    
    public void AsignarProyecto(string proyecto)
    {
        proyectos.Add(proyecto);
        Console.WriteLine($"✓ Proyecto '{proyecto}' asignado a {NombreCompleto}");
    }
    
    public override void MostrarInformacion()
    {
        Console.WriteLine($"👤 EMPLEADO: {NombreCompleto}");
        Console.WriteLine($"   ID: {Id}");
        Console.WriteLine($"   Edad: {Edad} años");
        Console.WriteLine($"   Departamento: {Departamento}");
        Console.WriteLine($"   Salario Base: {SalarioBase:C}");
        Console.WriteLine($"   Salario con Bonos: {SalarioConBonos:C}");
        Console.WriteLine($"   Años en empresa: {AñosEnEmpresa}");
        Console.WriteLine($"   Proyectos activos: {proyectos.Count}");
    }
    
    public string GenerarReporte()
    {
        return $"REPORTE - Empleado: {NombreCompleto}\n" +
               $"Departamento: {Departamento}\n" +
               $"Salario: {SalarioConBonos:C}\n" +
               $"Proyectos: {string.Join(", ", proyectos)}";
    }
}

// Clase Gerente (hereda de Empleado)
public class Gerente : Empleado
{
    private List<Empleado> equipo;
    public decimal BonoGerencial { get; set; }
    
    public int TamañoEquipo => equipo.Count;
    
    public Gerente(string id, string nombre, string apellido, DateTime fechaNacimiento,
                   string departamento, decimal salario, decimal bonoGerencial, 
                   DateTime? fechaContratacion = null)
        : base(id, nombre, apellido, fechaNacimiento, departamento, salario, fechaContratacion)
    {
        BonoGerencial = bonoGerencial;
        equipo = new List<Empleado>();
    }
    
    public void AgregarMiembro(Empleado empleado)
    {
        if (!equipo.Contains(empleado))
        {
            equipo.Add(empleado);
            Console.WriteLine($"✓ {empleado.NombreCompleto} agregado al equipo de {NombreCompleto}");
        }
    }
    
    public void MostrarEquipo()
    {
        Console.WriteLine($"\n=== Equipo de {NombreCompleto} ===");
        foreach (var miembro in equipo)
        {
            Console.WriteLine($"- {miembro.NombreCompleto} ({miembro.Departamento})");
        }
        Console.WriteLine($"Total: {TamañoEquipo} miembros");
    }
    
    public override void MostrarInformacion()
    {
        base.MostrarInformacion();
        Console.WriteLine($"   Bono Gerencial: {BonoGerencial:C}");
        Console.WriteLine($"   Tamaño del equipo: {TamañoEquipo}");
    }
}

// Clase Empresa
public class Empresa : IReportable
{
    public string Nombre { get; set; }
    private List<Empleado> empleados;
    private List<Gerente> gerentes;
    
    public int TotalEmpleados => empleados.Count + gerentes.Count;
    
    public Empresa(string nombre)
    {
        Nombre = nombre;
        empleados = new List<Empleado>();
        gerentes = new List<Gerente>();
    }
    
    public void ContratarEmpleado(Empleado empleado)
    {
        empleados.Add(empleado);
        Console.WriteLine($"✓ {empleado.NombreCompleto} contratado en {Nombre}");
    }
    
    public void ContratarGerente(Gerente gerente)
    {
        gerentes.Add(gerente);
        Console.WriteLine($"✓ {gerente.NombreCompleto} contratado como Gerente en {Nombre}");
    }
    
    public void MostrarOrganigrama()
    {
        Console.WriteLine($"\n╔════════════════════════════════════════╗");
        Console.WriteLine($"║  Organigrama - {Nombre,-23}║");
        Console.WriteLine($"╚════════════════════════════════════════╝\n");
        
        Console.WriteLine("GERENTES:");
        foreach (var gerente in gerentes)
        {
            gerente.MostrarInformacion();
            Console.WriteLine();
        }
        
        Console.WriteLine("\nEMPLEADOS:");
        foreach (var empleado in empleados)
        {
            empleado.MostrarInformacion();
            Console.WriteLine();
        }
        
        Console.WriteLine($"Total de personas: {TotalEmpleados}");
    }
    
    public decimal CalcularNominaTotal()
    {
        decimal total = 0;
        total += empleados.Sum(e => e.SalarioConBonos);
        total += gerentes.Sum(g => g.SalarioConBonos + g.BonoGerencial);
        return total;
    }
    
    public string GenerarReporte()
    {
        return $"REPORTE EMPRESA: {Nombre}\n" +
               $"Total Empleados: {TotalEmpleados}\n" +
               $"Nómina Total: {CalcularNominaTotal():C}";
    }
}

class Program
{
    static void Main(string[] args)
    {
        // Crear empresa
        Empresa empresa = new Empresa("TechCorp Solutions");
        
        // Crear gerente
        Gerente gerente = new Gerente("G001", "Ana", "Martínez",
            new DateTime(1985, 5, 15), "Tecnología", 5000, 1000);
        empresa.ContratarGerente(gerente);
        
        // Crear empleados
        Empleado emp1 = new Empleado("E001", "Carlos", "García",
            new DateTime(1990, 3, 20), "Tecnología", 3000);
        Empleado emp2 = new Empleado("E002", "Laura", "López",
            new DateTime(1992, 8, 10), "Tecnología", 3200);
        Empleado emp3 = new Empleado("E003", "Miguel", "Rodríguez",
            new DateTime(1988, 12, 5), "Ventas", 2800);
        
        empresa.ContratarEmpleado(emp1);
        empresa.ContratarEmpleado(emp2);
        empresa.ContratarEmpleado(emp3);
        
        // Asignar empleados al gerente
        Console.WriteLine();
        gerente.AgregarMiembro(emp1);
        gerente.AgregarMiembro(emp2);
        
        // Asignar proyectos
        Console.WriteLine();
        emp1.AsignarProyecto("Sistema ERP");
        emp1.AsignarProyecto("App Móvil");
        emp2.AsignarProyecto("Portal Web");
        
        // Mostrar información
        gerente.MostrarEquipo();
        empresa.MostrarOrganigrama();
        
        // Generar reportes
        Console.WriteLine("\n=== REPORTES ===\n");
        Console.WriteLine(empresa.GenerarReporte());
        Console.WriteLine("\n" + emp1.GenerarReporte());
    }
}
```

## Conclusiones

Estos ejemplos demuestran cómo combinar todos los conceptos de POO:

1. **Encapsulación**: Campos privados con propiedades públicas controladas
2. **Herencia**: Jerarquías de clases (Persona → Empleado → Gerente)
3. **Polimorfismo**: Métodos sobrescritos y colecciones polimórficas
4. **Abstracción**: Clases abstractas con métodos abstractos
5. **Interfaces**: Contratos como `IPrestable` e `IReportable`
6. **Propiedades**: Auto-implementadas y calculadas
7. **Constructores**: Con parámetros y encadenamiento

## Mejores Prácticas Aplicadas

- Validación de datos en constructores y propiedades
- Uso de propiedades calculadas para valores derivados
- Encapsulación adecuada de colecciones internas
- Métodos con responsabilidad única
- Nombres descriptivos y significativos
- Uso apropiado de modificadores de acceso
- Documentación a través de código claro y autoexplicativo
