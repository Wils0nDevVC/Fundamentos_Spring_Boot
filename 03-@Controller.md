
# @Controller: 

 - Es una notacion que permite mapear rutas web directamente.
 - Tecnicamente **Controller** en SpringBoot,  es una clase de Java que permite escuchar las peticiones Http de tipo Get, Post, Delete, Put. Decidir que hacer con ellas y devolver una respuesta. (Una Vista HTML, Json, etc.)
- Los Controllers como tal retornan vistas HTML.
- Las vistas son creadas en **resurce/template**.


### @GetMapping : 
    - Es una anotación de Spring que se usa dentro de un @RestController (o @Controller) para mapear peticiones HTTP GET a un método específico. Es un atajo de @RequestMapping(method = RequestMethod.GET).

    - Metodo Http de tipo Get, este captura la ruta (url)

### @RequestMapping 
    - Es la anotación base de Spring para mapear peticiones HTTP a controladores y métodos.
    - Permite configurar el verbo HTTP, la ruta, los headers, los parámetros aceptados y más.
    - Se puede usar de dos formas, a nivel de clase, para definir una ruta base común a todos los métodos del controlador y a nivel de metodo, especificando el verbo manualmente (ver ejemplo en el codigo)

### Model:
    - Es una interface que sirve como un contenedor de datos, lo usamos para pasar datos desde el controlador hacia la vista
    

```java
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

@Controller
@RequestMapping("cv") //-- De esta forma creamos una ruta base.
public class CvController {

    //Recordemos que cuando no hay ruta el servidor siempre busca el index.html por defecto
//    @GetMapping("")
//    public String index(){
//        return "index";
//    }
    // la ruta home renderiza el index, pero si no hubiera home, el servidor igual renderizaria este index.
//    @GetMapping("/home")
//    public String index(){
//        return "index";
//    }

    //definimos una url y retornamos su plantilla html de esta forma
//    @GetMapping("/helloworld")
//    public String helloworld(){
//        return "example";
//    }

//Tambien podemos definir multiples rutas, pero recordemos que el vacío siempre prioriza el index
//    @GetMapping({"/helloworld", "", "/home"})
//    public String helloworld(){
//        return "example";
//    }

    //Tambien podemos definir rutas con @RequestMapping, pero este mas se usa para definir rutas Bases.
//    @RequestMapping(value = "/helloworld", method = RequestMethod.GET)
//    public String helloworld(){
//        return "example";
//    }

     @GetMapping({"/index","", "/"})
    public String index(Model model){
        model.addAttribute("name", "Reynaldo");

        //Padando Objectos
        Person person = new Person("Wilson","Vasquez", "Ing Sistemas");

        model.addAttribute("person", person);

        return "index";
    }
}
```
### Plantilla thymeleaf
```html 
<!DOCTYPE html>
<html lang="es"
      xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Curriculum Vitae</title>
</head>
<body>
  <h1>Nuestro CV Index</h1>
  <p th:text="'Hola Recluter, ' + ${name} + ' ! '  "></p>
  <p th:text="'Nombre: ' + ${person.firstName} + ' ! '  "></p>
  <p th:text="'Apellido: ' + ${person.lastName} + ' ! '  "></p>
  <p th:text="'Profesion: ' + ${person.profession} + ' ! '  "></p>

</body>
</html>
````

