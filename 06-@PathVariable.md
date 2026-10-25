## @PathVariable 
- Es una anotación de Spring MVC que extrae valores directamente de la URL de la ruta (path) y los vincula a los parámetros de un método controlador. A diferencia de ```@RequestParam```, aquí el dato es parte de la estructura de la URL, no va después del ?.

```java

@GetMapping("/usuarios/{id}")
public Usuario getUsuario(@PathVariable Long id) {
    return usuarioService.buscarPorId(id);
}
```

- Si el cliente hace: GET /usuarios/42, Spring toma el 42 de la URL y lo asigna a la variable id.


### Cuando el nombre de la variable coincide

- Si el nombre del parámetro del método es igual al de la plantilla { }, no necesitas especificar nada más:

``` java
@GetMapping("/usuarios/{id}")
public Usuario getUsuario(@PathVariable Long id) { ... }  // funciona directo

```

### Cuando el nombre es distinto

- Si quieres usar un nombre de variable diferente, debes indicar el name (o value):

```java
@GetMapping("/usuarios/{idUsuario}")
public Usuario getUsuario(@PathVariable("idUsuario") Long id) { ... }
```
### Múltiples path variables
```java
@GetMapping("/usuarios/{idUsuario}/pedidos/{idPedido}")
public Pedido getPedido(
        @PathVariable Long idUsuario,
        @PathVariable Long idPedido) {
    return pedidoService.buscar(idUsuario, idPedido);
}
```

### PathVariable opcional

- Puedes hacerlo opcional con required = false, aunque es poco común (lo normal es que si falta, cambie toda la ruta):

```java
@GetMapping({"/usuarios/{id}", "/usuarios"})
public List<Usuario> getUsuario(@PathVariable(required = false) Long id) {
    if (id == null) {
        return usuarioService.listarTodos();
    }
    return List.of(usuarioService.buscarPorId(id));
}

```

### Capturar todas las variables en un Map
```java
@GetMapping("/usuarios/{id}/pedidos/{idPedido}")
public String metodo(@PathVariable Map<String, String> pathVars) {
    String id = pathVars.get("id");
    String idPedido = pathVars.get("idPedido");
    // ...
}

```

### Ejemplo combinando PathVariable y RequestVariable

```java
@GetMapping("/usuarios/{id}/pedidos")
public List<Pedido> getPedidosDeUsuario(
        @PathVariable Long id,                              // identifica al usuario
        @RequestParam(required = false) String estado) {     // filtra los pedidos
    // GET /usuarios/5/pedidos?estado=pendiente
    return pedidoService.buscar(id, estado);
}

```