### @Configuration

- Es una anotación de Spring Framework que indica que una clase contiene definiciones de objetos que serán administrados por el contenedor de Spring.

- Normalmente, estos objetos se declaran mediante métodos anotados con @Bean


```java
@Configuration
public class PaymentConfig {

    @Bean
    public PaymentValidator paymentValidator() {
        return new PaymentValidator();
    }

    @Bean
    public PaymentService paymentService(
            PaymentValidator paymentValidator) {

        return new PaymentService(paymentValidator);
    }
}
```
- Cuando Spring procesa esta configuración:

    1. Ejecuta paymentValidator().
    2. Registra el objeto retornado como un bean.
    3. Ejecuta paymentService(...).
    4. Inyecta el bean PaymentValidator en el parámetro.
    5. Registra el PaymentService resultante como otro bean.

- El contenedor queda conceptualmente así:

```
ApplicationContext
├── paymentValidator → PaymentValidator
└── paymentService   → PaymentService

```
### ¿Cómo funciona internamente?
Durante el inicio de la aplicación, Spring:
```
Detecta PaymentConfig
        ↓
Reconoce @Configuration
        ↓
Encuentra sus métodos @Bean
        ↓
Ejecuta esos métodos
        ↓
Registra los objetos retornados
        ↓
Administra sus dependencias y ciclo de vida

```

### ¿Qué problema resuelve?

- Permite registrar beans cuando:

    - La clase no puede anotarse con ```@Component``` o ```@Service.```
    - La clase pertenece a una librería externa.
    - Su creación requiere configuración personalizada.
    - Se necesita escoger entre diferentes implementaciones.
    - La creación del objeto depende de propiedades o parámetros específicos.

Por ejemplo, si BankApiClient pertenece a una librería externa, no podemos modificarlo para colocarle @Component. Podemos registrarlo manualmente:
```java
@Configuration
public class BankClientConfig {

    @Bean
    public BankApiClient bankApiClient() {
        return new BankApiClient(
                "https://api.banco.com",
                5_000
        );
    }
}
```
A partir de ese momento, BankApiClient puede inyectarse normalmente:
```java
@Service
public class DebtService {

    private final BankApiClient bankApiClient;

    public DebtService(BankApiClient bankApiClient) {
        this.bankApiClient = bankApiClient;
    }
}
```