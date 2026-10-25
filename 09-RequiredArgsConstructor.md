# @RequiredArgsConstructor 
es una anotación de Lombok que genera automáticamente un constructor con los campos obligatorios de la clase, es decir:

- Campos marcados como ```final```
- Campos marcados con ```@NonNul``` (aunque no sean final)


Ejemplo

Sin @RequiredArgsConstructor:
```java
public class UsuarioService {
    private final UsuarioRepository usuarioRepository;
    private final EmailService emailService;

    public UsuarioService(UsuarioRepository usuarioRepository, EmailService emailService) {
        this.usuarioRepository = usuarioRepository;
        this.emailService = emailService;
    }
}
```

con @RequiredArgsConstructor

```java 
@RequiredArgsConstructor
public class UsuarioService {
    private final UsuarioRepository usuarioRepository;
    private final EmailService emailService;
}
```

### ¿Por qué se usa tanto en Spring?

Es muy común combinarla con inyección de dependencias por constructor:

```java
@Service
@RequiredArgsConstructor
public class UsuarioService {
    private final UsuarioRepository usuarioRepository;
    private final EmailService emailService;

    public void crearUsuario(Usuario u) {
        usuarioRepository.save(u);
        emailService.enviarBienvenida(u);
    }
}
```