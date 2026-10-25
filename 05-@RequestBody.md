# @RequestBody

- @RequestBody indica que el contenido enviado en el cuerpo de una petición HTTP debe convertirse en un objeto Java.

```java
@PostMapping("/transferencias")
public TransferenciaResponse transferir(
        @RequestBody TransferenciaRequest request) {
    // ...
}
```

- Si el cliente envía 

```java
{
  "cuentaOrigen": "001-123",
  "cuentaDestino": "001-456",
  "importe": 500.00
}
```


## @RequestBody con validaciones

- ```@RequestBody``` convierte los datos, pero no aplica por sí solo las reglas del negocio. Para validar el DTO se utiliza ```@Valid```.


```java
public record TransferenciaRequest(

        @NotBlank
        String cuentaOrigen,

        @NotBlank
        String cuentaDestino,

        @NotNull
        @Positive
        BigDecimal importe
) {
}

//En el controller
@PostMapping
public TransferenciaResponse transferir(
        @Valid @RequestBody TransferenciaRequest request) {

    return transferenciaService.transferir(request);
}
```
- Cada anotación cumple una responsabilidad diferente:

```
@Valid @RequestBody TransferenciaRequest request
```
- ```@RequestBody```: convierte JSON en TransferenciaRequest.
-  ```@Valid```: ejecuta las validaciones declaradas en el DTO.