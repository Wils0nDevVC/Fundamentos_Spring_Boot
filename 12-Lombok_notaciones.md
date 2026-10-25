# Lombok - libreria de Java

Estas tres anotaciones pertenecen a Lombok, una librería Java que genera código en tiempo de compilación para evitar escribir código repetitivo (boilerplate).

- ```@Data```: Es una anotación "todo en uno". Genera automáticamente los métodos getters y setters para todos los atributos, el método toString(), los métodos equals() y hashCode(), y un constructor para los campos obligatorios (final). Sirve para crear clases de modelo o DTOs limpios en una sola línea.

- ```@AllArgsConstructor```: Genera automáticamente un constructor que recibe exactamente un parámetro por cada atributo declarado en la clase. Sirve para instanciar objetos pasando todos sus valores desde el inicio o para inyectar dependencias por constructor en componentes de Spring.

- ```@NoArgsConstructor```: Genera un constructor vacío (sin parámetros). Sirve para cumplir con los requisitos de frameworks como Spring, JPA/Hibernate o librerías de serialización JSON (Jackson), los cuales necesitan instanciar la clase sin argumentos antes de poblar sus datos.


# Lombok en arquitectura limpia 

- Lombok es una dependencia de compilación, no de ejecución (compile-only): Lombok no se empaqueta en el JAR final ni corre en el tiempo de ejecución de la aplicación. Es un procesador de anotaciones que le dice al compilador de Java cómo generar el bytecode ```(.class)```. Una vez compilado, el código del dominio queda escrito en Java puro (```getters, setters, constructores```).

- No invade la lógica de negocio: Lombok es solo "azúcar sintáctico" (syntactic sugar) para ahorrar código repetitivo (boilerplate). No impone infraestructura, patrones de entrada/salida ni acoplamiento a frameworks de base de datos o frameworks web.

- Mantiene el código del dominio limpio y legible: Sin Lombok, una entidad de dominio con 10 campos requeriría cientos de líneas de código repetitivo de ```getters, setters, equals() y hashCode()```, lo que opaca la verdadera lógica de negocio.