
# JSON Converter en C#

## ¿Qué significa serializar y deserializar?

- **Serializar**: Convertir un objeto de C# (clase, lista, etc.) en una cadena JSON.  

- **Deserializar**: Hacer lo contrario: convertir una cadena JSON en un objeto de C# que se puede manipular.

---

## ¿Qué es un `JsonConverter`?

Un **JsonConverter** es una clase de .NET que permite controlar cómo se serializa o deserializa un tipo de objeto en JSON usando `System.Text.Json`.

### Se utiliza cuando:

- Se quiere personalizar cómo se escribe/lee un objeto en JSON.
- El formato JSON no coincide directamente con la estructura del objeto.
- Se quiere controlar errores o formatos específicos (como fechas, enums, etc).

---

## ¿Cuándo se usa?

- Para formatos personalizados (ej. fechas, booleanos como `"sí"`/`"no"`).
- Para ignorar propiedades según lógica personalizada.
- Para transformar estructuras anidadas o JSON no estándar.
- Para manejar errores de deserialización con más control.

---

## ¿Cómo se usa?

1. Crear una clase que herede de `JsonConverter<T>`.
2. Implementar los métodos:
   - `Read()` → para deserializar (de JSON a objeto).
   - `Write()` → para serializar (de objeto a JSON).
3. Aplicar el convertidor usando `[JsonConverter(typeof(MiConverter))]` o globalmente en `JsonSerializerOptions`.

---

## Ejemplo básico de JsonConverter personalizado

```
public class BoolSiNoConverter : JsonConverter<bool>
{
    public override bool Read(ref Utf8JsonReader reader, Type typeToConvert, JsonSerializerOptions options)
    {
        var value = reader.GetString()?.ToLower();
        return value == "sí" || value == "si" || value == "yes";
    }

    public override void Write(Utf8JsonWriter writer, bool value, JsonSerializerOptions options)
    {
        writer.WriteStringValue(value ? "sí" : "no");
    }
}

public class Usuario
{
    public string Nombre { get; set; }

    [JsonConverter(typeof(BoolSiNoConverter))]
    public bool Activo { get; set; }
}
```

`Resultado JSON`:

```
{ "nombre": "Jona", "activo": "sí" }
```

`Usos comunes`: 

| Caso de uso                         | Ejemplo                                           |
| :---                                | :---                                              |
| Serializar fechas en formato ISO    | Personalizar `DateTimeConverter`                  |
| Enum con texto personalizado        | Convertir enum a `"Activo"` / `"Inactivo"`        |
| Ignorar valores nulos complejos     | Convertidores que filtran propiedades al escribir |
| Deserializar campos booleanos raros | `"Y"/"N"`, `"1"/"0"`, `"Yes"/"No"`                |




