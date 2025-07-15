
## ¿Qué es una `API`?
_Application Programming Interface_  / _Interfaz de Programación de Aplicaciones_

Funciona como un intermediario, permitiendo que una aplicación solicite datos o funcionalidades a otra aplicación, sin necesidad de conocer los detalles internos de su funcionamiento. 

**`Analogía`:**
API como un camarero en un restaurante

El camarero es la API entre tú (el cliente) y la cocina (el sistema).  
Tú haces el pedido, el camarero lo lleva a la cocina, y luego te trae la comida.  
- No entras a la cocina ni ves cómo preparan tu plato.
- Solo usas un “servicio” para obtener algo.

![analogia](/CSharp_API/img/analogia.png)

---

## `Tipos de API`

| Tipo de API              | Acceso                                        | Uso Común                                           |
| :---                     | :---                                          | :---                                                |
| **API Pública**          | Abierta a cualquier desarrollador             | Aplicaciones móviles, mashups, servicios SaaS       |
| **API Privada**          | Solo uso interno de la organización           | Comunicación entre sistemas internos                |
| **API de Socios**        | Acceso restringido a socios autorizados       | Integraciones B2B (entre empresas)                  |
| **API REST**             | Abierta o restringida según configuración     | Aplicaciones web y móviles, microservicios          |
| **API SOAP**             | Generalmente restringida (empresarial)        | Servicios financieros, sistemas críticos            |
| **API GraphQL**          | Variable (según configuración del servidor)   | Apps modernas con consumo optimizado de datos       |
| **API WebSocket**        | Normalmente privada o autenticada             | Aplicaciones en tiempo real (chat, trading, juegos) |

