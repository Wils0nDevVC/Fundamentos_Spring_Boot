# JDCB
- Es la API estándar de Java para comunicarse con bases de datos relacionales.

# JdbcTemplate
- Es una clase de Spring que utiliza JDBC internamente y elimina gran parte del código repetitivo.
```
Aplicación
    ↓
JdbcTemplate (Spring)
    ↓
JDBC (Java)
    ↓
Driver JDBC
    ↓
Base de datos
```

## 1. JDBC
 - JDBC significa Java Database Connectivity. Permite:
    - Abrir conexiones.
    - Ejecutar SQL.
    - Enviar parámetros.
    - Leer resultados.
    - Manejar transacciones.
    - Cerrar recursos. 
- Ejemplo: consultar una deuda utilizando JDBC directamente.
```java
public Deuda buscarDeuda(Long clienteId) throws SQLException {

    String sql = """
        SELECT id, cliente_id, importe, estado
        FROM deuda
        WHERE cliente_id = ?
        """;

    try (
        Connection connection = dataSource.getConnection();
        PreparedStatement statement = connection.prepareStatement(sql)
    ) {
        statement.setLong(1, clienteId);

        try (ResultSet resultSet = statement.executeQuery()) {

            if (resultSet.next()) {
                return new Deuda(
                    resultSet.getLong("id"),
                    resultSet.getLong("cliente_id"),
                    resultSet.getBigDecimal("importe"),
                    resultSet.getString("estado")
                );
            }

            return null;
        }
    }
}
```
- JDBC obliga a controlar manualmente:

    - Connection
    - PreparedStatement
    - ResultSet
    - Cierre de recursos
    - SQLException
    - Conversión de cada fila a un objeto Java
- El ```try-with-resources``` cierra automáticamente los recursos, pero el código sigue siendo bastante extenso.

## 2. JdbcTemplate

- ```JdbcTemplate``` pertenece a Spring Framework, concretamente al módulo Spring JDBC.

- Internamente continúa utilizando JDBC, pero se encarga de:
    - Obtener la conexión desde el DataSource.
    - Crear y ejecutar el PreparedStatement.
    - Asignar los parámetros.
    - Recorrer el ResultSet.
    - Cerrar los recursos.
    - Traducir las SQLException a excepciones de Spring.

- El desarrollador se concentra principalmente en:
    - Escribir el SQL.
    - Proporcionar los parámetros.
    - Convertir cada fila en un objeto.

- El mismo ejemplo anterior queda así:
```java
@Repository
public class DeudaRepository {

    private final JdbcTemplate jdbcTemplate;

    public DeudaRepository(JdbcTemplate jdbcTemplate) {
        this.jdbcTemplate = jdbcTemplate;
    }

    public Deuda buscarPorCliente(Long clienteId) {

        String sql = """
            SELECT id, cliente_id, importe, estado
            FROM deuda
            WHERE cliente_id = ?
            """;

        return jdbcTemplate.queryForObject(
            sql,
            (resultSet, rowNum) -> new Deuda(
                resultSet.getLong("id"),
                resultSet.getLong("cliente_id"),
                resultSet.getBigDecimal("importe"),
                resultSet.getString("estado")
            ),
            clienteId
        );
    }
}
```
- El ```RowMapper<Deuda>```. Su responsabilidad es transformar una fila del ResultSet en un objeto Java.

## 3. Comparación directa

| Aspecto                  | JDBC           | JdbcTemplate          |
| ------------------------ | -------------- | --------------------- |
| Pertenece a              | Java           | Spring Framework      |
| Ejecuta SQL directamente | Sí             | Sí                    |
| Usa JDBC internamente    | Es JDBC        | Sí                    |
| Manejo de conexiones     | Manual         | Automático            |
| Cierre de recursos       | Manual         | Automático            |
| Lectura del `ResultSet`  | Manual         | Mediante `RowMapper`  |
| Excepciones              | `SQLException` | `DataAccessException` |
| Genera SQL               | No             | No                    |
| Es un ORM                | No             | No                    |
| Control sobre el SQL     | Alto           | Alto                  |
| Código repetitivo        | Mucho          | Menor                 |

- Una aclaración importante:

```JdbcTemplate no es una alternativa a JDBC en sentido estricto. Es una abstracción construida sobre JDBC.```