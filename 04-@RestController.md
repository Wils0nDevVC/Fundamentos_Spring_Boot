
# @RestController
- Es una anotación de Spring que combina ```@Controller``` y ```@ResponseBody```. Marca una clase como controlador REST, donde cada método devuelve directamente datos (JSON o XML) en el cuerpo de la respuesta HTTP, sin necesidad de resolver una vista.

- ```@RestController``` no convierte por sí mismo los objetos a JSON o XML; Spring utiliza los ```HTTP Message Converters``` disponibles. En Spring Boot, normalmente Jackson serializa los objetos a JSON.

### ¿Para qué sirve? 

- Sirve para exponer endpoints   de una API REST que entregan datos a clientes como:

    - Aplicaciones Angular, React o móviles.
    - Otros sistemas o microservicios.
    - Postman.
    - Power BI u otros consumidores de APIs.
- Normalmente, Spring Boot serializa los objetos Java a JSON.

### La diferencia con @Controller es esta:

| Anotación         | Uso habitual                   | Valor retornado                           |
| ----------------- | ------------------------------ | ----------------------------------------- |
| `@Controller`     | Aplicaciones MVC tradicionales | Nombre de una vista HTML                  |
| `@RestController` | APIs REST                      | Datos en el cuerpo HTTP, normalmente JSON |


**En resumen: @Controller = vistas (HTML), @RestController = datos (JSON/XML), porque ya trae @ResponseBody incorporado.**

#### Ejemplo @Controller
 - Con ```@Controller```, para devolver datos directamente tendrías que agregar ```@ResponseBody```:
```java
@Controller
public class CvController {

    @ResponseBody
    @GetMapping("/api/cv")
    public Person getPerson() {
        return new Person("Wilson", "Vasquez", "Ing. de Sistemas");
    }
}
```
- Con ```@RestController```, ```@ResponseBody``` se aplica implícitamente a todos los métodos:
```java
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;


@RestController
@RequestMapping("api")
public class CvApiController {

    @GetMapping("/cv")
    public Person getPerson(){
        return new Person("Wilson","Vasquez", "Ing Sistemas");
        
    }

}
```