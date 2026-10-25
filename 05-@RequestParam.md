
## @RequestParam 
- @RequestParam es una anotación de Spring MVC que permite obtener parámetros enviados en la URL de una petición HTTP.

- Normalmente se utiliza para recibir filtros, criterios de búsqueda, paginación o valores opcionales.

### Ejemplo básico
- Petición:
```
GET /deudas?clienteId=25
```
- controller
```java
@GetMapping("/deudas")
public List<Deuda> buscarDeudas(
        @RequestParam Long clienteId) {

    return deudaService.buscarPorCliente(clienteId);
}
```

- Spring toma el valor textual "25", lo convierte a Long y lo asigna a clienteId.

## Nombre explícito

- Puedes indicar cómo se llama el parámetro HTTP:
```java
@GetMapping("/deudas")
public List<Deuda> buscarDeudas(
        @RequestParam(name = "clienteId") Long id) {

    return deudaService.buscarPorCliente(id);
}
```

## Parámetros obligatorios

- Por defecto, ```@RequestParam``` es obligatorio:
- Si se llama al endpoint sin el parámetro, arrojara un error de ```400 Bad Request```
```java
@RequestParam Long clienteId
```
- Porque internamente se comporta como:``` @RequestParam(required = true) ```

## Parámetros opcionales

- Usando ```required = false```

```java
@GetMapping("/movimientos")
public List<Movimiento> buscar(
        @RequestParam(required = false) String tipo) {

    return movimientoService.buscar(tipo);
}
```

- Ambas peticiones son válidas:

```
GET /movimientos
GET /movimientos?tipo=DEBITO
```
- Si no se envía tipo, su valor será null.
- Para validar esto y no tener problema se debe usar la clase ```Optional```
```java
@GetMapping("/movimientos")
public List<Movimiento> buscar(
        @RequestParam Optional<String> tipo) {

    return movimientoService.buscar(tipo);
}
/// puede procesarse: 
String tipoMovimiento = tipo.orElse("TODOS");
```
- Tambien se puede usar ```defaultValue```
```java
@GetMapping("/movimientos")
public List<Movimiento> buscar(
        @RequestParam(defaultValue = "TODOS") String tipo) {

    return movimientoService.buscar(tipo);
}
```


## Ejemplo real: consulta de movimientos

```java 
@RestController
@RequestMapping("/api/movimientos")
public class MovimientoController {

    private final MovimientoService movimientoService;

    public MovimientoController(MovimientoService movimientoService) {
        this.movimientoService = movimientoService;
    }

    @GetMapping
    public List<MovimientoResponse> buscar(
            @RequestParam Long cuentaId,
            @RequestParam(required = false) String tipo,
            @RequestParam(defaultValue = "0") int pagina,
            @RequestParam(defaultValue = "20") int limite) {

        return movimientoService.buscar(
                cuentaId,
                tipo,
                pagina,
                limite
        );
    }
}

```