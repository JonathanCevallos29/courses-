
## ¿Cómo encriptar información en C#?

En C#, encriptar (o proteger) información es esencial para datos sensibles como contraseñas, tokens, archivos de configuración o información personal.

.NET ofrece diferentes formas de proteger o cifrar datos, según el escenario. Uno de los enfoques más comunes es usar el **sistema de protección de datos de ASP.NET Core**, que se basa en el uso de la interfaz `IDataProtectionProvider`.

---

## ¿Qué es la interfaz `IDataProtectionProvider`?

Es una interfaz del sistema de **protección de datos** de ASP.NET Core que permite crear objetos que protegen (encriptan) y desprotegen (desencriptan) información.

Forma parte del paquete:  
```
Microsoft.AspNetCore.DataProtection
```

Para instalarlo:
```
dotnet add package Microsoft.AspNetCore.DataProtection
```

## ¿Cómo funciona?

1. Registrar el servicio con AddDataProtection() en Program.cs.
2. Inyectar IDataProtectionProvider.
3. Usar el método CreateProtector(string purpose) para generar un protector.
4. Usar **.Protect()** para cifrar y **.Unprotect()** para descifrar datos











