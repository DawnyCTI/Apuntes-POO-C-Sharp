# Interfaces

## ¿Qué es una Interfaz?

Una **interfaz** es un contrato que define un conjunto de métodos y propiedades que una clase debe implementar, sin especificar cómo se implementan. Define "qué" debe hacer una clase, pero no "cómo" lo hace.

## Características de las Interfaces

- Se declaran con la palabra clave `interface`
- No pueden ser instanciadas directamente
- No contienen implementación (excepto en C# 8.0+)
- Todos los miembros son públicos por defecto
- Una clase puede implementar múltiples interfaces
- Permiten lograr herencia múltiple de comportamiento

## Sintaxis Básica

```csharp
// Definición de interfaz
public interface IVehiculo
{
    // Propiedades
    string Marca { get; set; }
    string Modelo { get; set; }
    
    // Métodos
    void Arrancar();
    void Detener();
    void Acelerar(int velocidad);
}
```

## Implementación de Interfaces

```csharp
public class Coche : IVehiculo
{
    public string Marca { get; set; }
    public string Modelo { get; set; }
    
    public void Arrancar()
    {
        Console.WriteLine($"{Marca} {Modelo} está arrancando...");
    }
    
    public void Detener()
    {
        Console.WriteLine($"{Marca} {Modelo} se ha detenido.");
    }
    
    public void Acelerar(int velocidad)
    {
        Console.WriteLine($"{Marca} {Modelo} acelerando a {velocidad} km/h");
    }
}
```

## Múltiples Interfaces

Una clase puede implementar múltiples interfaces:

```csharp
public interface IVolador
{
    void Despegar();
    void Aterrizar();
    int AltitudMaxima { get; }
}

public interface INavegable
{
    void Navegar();
    void Anclar();
}

// Clase que implementa múltiples interfaces
public class Hidroavion : IVehiculo, IVolador, INavegable
{
    public string Marca { get; set; }
    public string Modelo { get; set; }
    public int AltitudMaxima { get; } = 10000;
    
    // Implementación de IVehiculo
    public void Arrancar()
    {
        Console.WriteLine("Motor del hidroavión encendido");
    }
    
    public void Detener()
    {
        Console.WriteLine("Motor del hidroavión apagado");
    }
    
    public void Acelerar(int velocidad)
    {
        Console.WriteLine($"Acelerando a {velocidad} nudos");
    }
    
    // Implementación de IVolador
    public void Despegar()
    {
        Console.WriteLine("Hidroavión despegando del agua...");
    }
    
    public void Aterrizar()
    {
        Console.WriteLine("Hidroavión aterrizando en el agua...");
    }
    
    // Implementación de INavegable
    public void Navegar()
    {
        Console.WriteLine("Navegando sobre el agua");
    }
    
    public void Anclar()
    {
        Console.WriteLine("Anclando en el puerto");
    }
}
```

## Convenciones de Nomenclatura

Por convención, los nombres de interfaces en C# comienzan con la letra "I":

```csharp
public interface IRepositorio { }
public interface IServicio { }
public interface IConectable { }
public interface IComparable { }
```

## Ejemplo Completo: Sistema de Notificaciones

```csharp
using System;
using System.Collections.Generic;

// Interfaz para servicios de notificación
public interface INotificacion
{
    void EnviarNotificacion(string destinatario, string mensaje);
    bool VerificarDisponibilidad();
    string ObtenerTipo();
}

// Interfaz para servicios que requieren autenticación
public interface IAutenticable
{
    bool Autenticar(string usuario, string contraseña);
    void CerrarSesion();
}

// Implementación: Email
public class NotificacionEmail : INotificacion, IAutenticable
{
    private bool autenticado = false;
    
    public void EnviarNotificacion(string destinatario, string mensaje)
    {
        if (!autenticado)
        {
            Console.WriteLine("Error: Debe autenticarse primero");
            return;
        }
        
        Console.WriteLine($"📧 Enviando email a {destinatario}");
        Console.WriteLine($"Mensaje: {mensaje}");
        Console.WriteLine("Email enviado exitosamente\n");
    }
    
    public bool VerificarDisponibilidad()
    {
        // Simular verificación de servidor
        return true;
    }
    
    public string ObtenerTipo()
    {
        return "Email";
    }
    
    public bool Autenticar(string usuario, string contraseña)
    {
        // Simular autenticación
        Console.WriteLine($"Autenticando usuario: {usuario}");
        autenticado = true;
        return true;
    }
    
    public void CerrarSesion()
    {
        autenticado = false;
        Console.WriteLine("Sesión cerrada\n");
    }
}

// Implementación: SMS
public class NotificacionSMS : INotificacion
{
    public void EnviarNotificacion(string destinatario, string mensaje)
    {
        Console.WriteLine($"📱 Enviando SMS a {destinatario}");
        Console.WriteLine($"Mensaje: {mensaje}");
        Console.WriteLine("SMS enviado exitosamente\n");
    }
    
    public bool VerificarDisponibilidad()
    {
        return true;
    }
    
    public string ObtenerTipo()
    {
        return "SMS";
    }
}

// Implementación: Push Notification
public class NotificacionPush : INotificacion
{
    public void EnviarNotificacion(string destinatario, string mensaje)
    {
        Console.WriteLine($"🔔 Enviando notificación push a {destinatario}");
        Console.WriteLine($"Mensaje: {mensaje}");
        Console.WriteLine("Notificación push enviada exitosamente\n");
    }
    
    public bool VerificarDisponibilidad()
    {
        return true;
    }
    
    public string ObtenerTipo()
    {
        return "Push";
    }
}

// Servicio que gestiona notificaciones
public class ServicioNotificaciones
{
    private List<INotificacion> serviciosNotificacion;
    
    public ServicioNotificaciones()
    {
        serviciosNotificacion = new List<INotificacion>();
    }
    
    public void RegistrarServicio(INotificacion servicio)
    {
        if (servicio.VerificarDisponibilidad())
        {
            serviciosNotificacion.Add(servicio);
            Console.WriteLine($"Servicio {servicio.ObtenerTipo()} registrado\n");
        }
    }
    
    public void EnviarNotificacionATodos(string destinatario, string mensaje)
    {
        Console.WriteLine("=== ENVIANDO NOTIFICACIONES ===\n");
        foreach (var servicio in serviciosNotificacion)
        {
            servicio.EnviarNotificacion(destinatario, mensaje);
        }
    }
    
    public void EnviarPorTipo(string tipo, string destinatario, string mensaje)
    {
        foreach (var servicio in serviciosNotificacion)
        {
            if (servicio.ObtenerTipo() == tipo)
            {
                servicio.EnviarNotificacion(destinatario, mensaje);
                break;
            }
        }
    }
}

class Program
{
    static void Main(string[] args)
    {
        // Crear servicios de notificación
        var email = new NotificacionEmail();
        var sms = new NotificacionSMS();
        var push = new NotificacionPush();
        
        // Autenticar servicio de email
        if (email is IAutenticable autenticable)
        {
            autenticable.Autenticar("usuario@example.com", "password123");
        }
        
        // Registrar servicios
        var servicio = new ServicioNotificaciones();
        servicio.RegistrarServicio(email);
        servicio.RegistrarServicio(sms);
        servicio.RegistrarServicio(push);
        
        // Enviar notificaciones
        servicio.EnviarNotificacionATodos("Juan Pérez", "¡Bienvenido a nuestra plataforma!");
        
        Console.WriteLine("\n=== ENVIANDO SOLO POR EMAIL ===\n");
        servicio.EnviarPorTipo("Email", "maria@example.com", "Recordatorio de reunión");
    }
}
```

## Interfaces vs Clases Abstractas

### Usa Interfaces cuando:
- Defines un contrato que puede ser implementado por clases no relacionadas
- Necesitas herencia múltiple de comportamiento
- Quieres definir capacidades que pueden ser compartidas por diferentes jerarquías

### Usa Clases Abstractas cuando:
- Tienes código común que compartir entre clases relacionadas
- Necesitas definir campos no estáticos o no constantes
- Quieres proporcionar implementación predeterminada para algunos métodos

## Interfaces Comunes en .NET

```csharp
// IComparable - Para comparar objetos
public class Persona : IComparable<Persona>
{
    public string Nombre { get; set; }
    public int Edad { get; set; }
    
    public int CompareTo(Persona otra)
    {
        return this.Edad.CompareTo(otra.Edad);
    }
}

// IDisposable - Para liberar recursos
public class ConexionBD : IDisposable
{
    public void Dispose()
    {
        // Liberar recursos
        Console.WriteLine("Recursos liberados");
    }
}

// IEnumerable - Para iterar colecciones
public class ColeccionPersonalizada : IEnumerable<int>
{
    private int[] numeros = { 1, 2, 3, 4, 5 };
    
    public IEnumerator<int> GetEnumerator()
    {
        foreach (var num in numeros)
        {
            yield return num;
        }
    }
    
    System.Collections.IEnumerator System.Collections.IEnumerable.GetEnumerator()
    {
        return GetEnumerator();
    }
}
```

## Implementación Explícita de Interfaces

Cuando una clase implementa múltiples interfaces con miembros del mismo nombre:

```csharp
public interface IImpresoraA
{
    void Imprimir();
}

public interface IImpresoraB
{
    void Imprimir();
}

public class ImpresoraMultifuncional : IImpresoraA, IImpresoraB
{
    // Implementación explícita
    void IImpresoraA.Imprimir()
    {
        Console.WriteLine("Imprimiendo con Impresora A");
    }
    
    void IImpresoraB.Imprimir()
    {
        Console.WriteLine("Imprimiendo con Impresora B");
    }
}

// Uso
ImpresoraMultifuncional impresora = new ImpresoraMultifuncional();
((IImpresoraA)impresora).Imprimir(); // Imprimiendo con Impresora A
((IImpresoraB)impresora).Imprimir(); // Imprimiendo con Impresora B
```

## Ventajas de las Interfaces

1. **Flexibilidad**: Permite múltiples implementaciones del mismo contrato
2. **Desacoplamiento**: Reduce dependencias entre clases
3. **Testabilidad**: Facilita la creación de mocks para pruebas
4. **Extensibilidad**: Fácil agregar nuevas funcionalidades
5. **Polimorfismo**: Permite tratar diferentes clases de manera uniforme

## Mejores Prácticas

- Mantén las interfaces pequeñas y enfocadas (Interface Segregation Principle)
- Usa nombres descriptivos que indiquen capacidades
- Prefiere muchas interfaces pequeñas a pocas grandes
- Programa contra interfaces, no implementaciones
- Documenta el contrato esperado de cada método
