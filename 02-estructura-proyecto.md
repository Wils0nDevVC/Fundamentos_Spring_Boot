# Estructura de un Proyecto SpringBoot
- **src** : es el directorio donde se encuentra el codigo fuente
- **src/main/java** : Aqui es donde vive la aplicacion como tal
- **resources** : Contiene todos los recursos que no son código Java, pero que la aplicación necesita en tiempo de ejecución.
- **resource/application.properties** : Es el archivo de configuración principal de Spring Boot. Se usa básicamente para crear variables de configuracion 
- **resource/static** : Aquí se colocan los archivos estáticos que el navegador puede solicitar directamente.
- **resurce/templates** : Si utilizas un motor de plantillas como Thymeleaf, aquí van las vistas HTML.
- **Target :**  Es la carpeta generada por maven al compilar el proyecto, tiene todos los .class que necesitamos nuestras clases, recursos empaquetados, archivos temporales,etc.
- **pom.xml :** es el archivo más importante de un proyecto Maven. Es el equivalente al package.json en Node.js o al build.gradle en 
Gradle
  - Su función es decirle a Maven:
      - Qué proyecto está construyendo.
      - Qué versión de Java usar.
      - Qué dependencias descargar.
      - Cómo compilar y empaquetar la aplicación.
      - Qué plugins utiliszar.
