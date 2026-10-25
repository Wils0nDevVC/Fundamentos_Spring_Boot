# @ModelAttributed : 

    - Es una anotación de Spring MVC (Spring Framework) que se usa para vincular datos entre el modelo de una vista y los parámetros de un método controlador. 
    
## Sirve principalmente para dos cosas:
1. Vincular datos de un formulario a un objeto Java.
    - Cuando un formulario HTML envía datos, @ModelAttribute toma esos parámetros y los mapea automáticamente a los campos de un objeto (POJO).

    ```java
    @PostMapping("/registrar")
    public String registrarUsuario(@ModelAttribute("usuario") Usuario usuario) {
        // Spring toma los campos del formulario (nombre, email, etc.)
        // y los asigna automáticamente al objeto "usuario"
        usuarioService.guardar(usuario);
        return "confirmacion";
    }
    ```
    - Si el formulario tiene inputs name="nombre" y name="email", Spring llama automáticamente a usuario.setNombre(...) y usuario.setEmail(...).
    
2. Agregar atributos al modelo antes de renderizar una vista.

    - Cuando se usa en un método sin parámetros de retorno o como método separado, sirve para precargar datos que estarán disponibles en todas las vistas de ese controlador (por ejemplo, para llenar un ```<select>``` con opciones).
    - Esto es equivalente a que Spring haga internamente: ``` model.addAttribute("paises", List.of("Perú", "Chile", "Colombia")); ```.
    - Es como si dijeras ```Lis<String> paises = List.of("Perú", "Chile", "Colombia")```

    ```java

    @ModelAttribute("paises")
    public List<String> cargarPaises() {
    
    return List.of("Perú", "Chile", "Colombia");
    }

    ```
- Este método se ejecuta antes de cualquier @RequestMapping del controlador, y el resultado (paises) queda disponible automáticamente en el modelo para la vista (Thymeleaf, JSP, etc.).

| Campo | Tipo | Descripción |
|-------|------|-------------|
| id | bigint | Identificador único |
| name | varchar | Nombre del usuario |
| email | varchar | Correo electrónico |
| created_at | timestamp | Fecha de creación |

| `@ModelAttribute` | `@RequestBody` |
|---|---|
| Datos de formularios (`application/x-www-form-urlencoded`, `multipart/form-data`) | Datos enviados en el cuerpo de la petición, normalmente JSON |
| Vincula parámetros del request a los campos de un objeto | Deserializa el cuerpo completo de la petición a un objeto |
| Común en aplicaciones MVC con formularios HTML | Común en APIs REST |
| Usa parámetros/form fields para construir el objeto | Usa un `HttpMessageConverter` (por ejemplo, Jackson) para convertir JSON → objeto Java |
| Ejemplo: `nombre=Juan&edad=25` | Ejemplo: `{"nombre":"Juan","edad":25}` |