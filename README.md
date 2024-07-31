
# productos-pro

## Descripción General
`productos-pro` es un proyecto de demostración para AgendaPro, diseñado para gestionar productos con funcionalidades como la creación, actualización, eliminación y búsqueda de productos. Este proyecto está construido utilizando Spring Boot, con varias integraciones para seguridad, persistencia de datos y funcionalidades web.

## Tabla de Contenidos
1. [Tecnologías Utilizadas](#tecnologías-utilizadas)
2. [Estructura del Proyecto](#estructura-del-proyecto)
3. [Características Clave](#características-clave)
4. [Arquitectura](#arquitectura)
5. [Configuración e Instalación](#configuración-e-instalación)
6. [Uso](#uso)
7. [Endpoints](#endpoints)
8. [Manejo de Excepciones](#manejo-de-excepciones)
9. [Contribuciones](#contribuciones)
10. [Licencia](#licencia)

## Tecnologías Utilizadas
- **Java 17**
- **Spring Boot 3.3.2**
  - Spring Boot Starter Data JPA
  - Spring Boot Starter Security
  - Spring Boot Starter Web
  - Spring Boot DevTools
- **Base de Datos H2** (runtime)
- **Project Lombok**
- **MapStruct** (1.5.5.Final)
- **Mockito** (4.3.1)
- **Maven**
- **Docker**

## Estructura del Proyecto
\`\`\`plaintext
src/main/java/com/prodpro/admproducts
├── config
│   ├── RestTemplateConfig.java
│   └── SecurityConfig.java
├── controller
│   └── ProductController.java
├── exceptionsmanager
│   ├── GlobalExceptionHandler.java
│   └── exceptions
│       ├── ErrorDetails.java
│       ├── GeneralException.java
│       └── ProductNotFoundException.java
├── mapper
│   └── ProductMapper.java
├── model
│   └── Product.java
├── repository
│   └── ProductRepository.java
└── service
    └── ProductService.java
\`\`\`

## Características Clave
- **Gestión de Productos**: Crear, actualizar, eliminar y buscar productos.
- **Seguridad**: Autenticación básica con Spring Security.
- **Persistencia de Datos**: Uso de JPA y base de datos H2 para almacenamiento de datos.
- **Manejo de Excepciones**: Manejo centralizado de excepciones mediante `GlobalExceptionHandler`.

## Arquitectura
El diagrama de arquitectura a continuación muestra los componentes principales y su interacción:

![Arquitectura](./docs/arquitectura.png)

### Descripción del Diagrama
1. **Controlador (`ProductController`)**: Gestiona las solicitudes HTTP y coordina con el servicio de productos.
2. **Servicio (`ProductService`)**: Contiene la lógica de negocio para la gestión de productos.
3. **Repositorio (`ProductRepository`)**: Interactúa con la base de datos para las operaciones CRUD.
4. **Manejo de Excepciones (`GlobalExceptionHandler`)**: Captura y maneja las excepciones de manera centralizada.
5. **Configuraciones**: Configuraciones de seguridad (`SecurityConfig`) y plantillas REST (`RestTemplateConfig`).

## Configuración e Instalación
1. Clonar el repositorio:
    \`\`\`bash
    git clone https://github.com/tu_usuario/productos-pro.git
    \`\`\`
2. Navegar al directorio del proyecto:
    \`\`\`bash
    cd productos-pro
    \`\`\`
3. Construir el proyecto con Maven:
    \`\`\`bash
    mvn clean install
    \`\`\`
4. Ejecutar la aplicación:
    \`\`\`bash
    mvn spring-boot:run
    \`\`\`

### Usando Docker
1. Construir la imagen Docker:
    \`\`\`bash
    docker build -t productos-pro .
    \`\`\`
2. Ejecutar el contenedor:
    \`\`\`bash
    docker run -p 8080:8080 productos-pro
    \`\`\`

## Dockerfile
\`\`\`dockerfile
# Etapa 1: Construcción
FROM maven:3.8.5-openjdk-17 AS build

WORKDIR /app

# Copia el archivo pom.xml y descarga las dependencias necesarias
COPY pom.xml .
RUN mvn dependency:go-offline

# Copia el resto de los archivos del proyecto y construye el JAR
COPY src ./src
RUN mvn clean package

# Etapa 2: Ejecución
FROM openjdk:17-jdk-slim

WORKDIR /app

# Copia el JAR construido en la etapa de construcción
COPY --from=build /app/target/productos-pro-0.0.1-SNAPSHOT.jar /app/productos-pro-0.0.1-SNAPSHOT.jar

EXPOSE 8080

ENTRYPOINT ["java", "-jar", "/app/productos-pro-0.0.1-SNAPSHOT.jar"]
\`\`\`

## Uso
Una vez que la aplicación esté en ejecución, puede acceder a los endpoints definidos para gestionar los productos.

## Endpoints
- \`POST /api/product\`: Crear un nuevo producto.
- \`PUT /api/product/{id}\`: Actualizar un producto existente.
- \`DELETE /api/product/{id}\`: Eliminar un producto.
- \`GET /api/product\`: Listar todos los productos.
- \`GET /api/product/{id}\`: Obtener un producto por ID.
- \`GET /api/product/search?nombre={nombre}\`: Buscar productos por nombre.

## Manejo de Excepciones
El proyecto incluye un manejo centralizado de excepciones utilizando `GlobalExceptionHandler`, que maneja las siguientes excepciones:
- \`ProductNotFoundException\`
- \`GeneralException\`
- Excepciones generales (\`Exception\`)

## Contribuciones
Las contribuciones son bienvenidas. Por favor, siga estos pasos:
1. Haga un fork del repositorio.
2. Cree una rama para su característica (\`git checkout -b feature/nueva-caracteristica\`).
3. Realice un commit de sus cambios (\`git commit -am 'Añadir nueva característica'\`).
4. Haga push a la rama (\`git push origin feature/nueva-caracteristica\`).
5. Cree un nuevo Pull Request.

## Licencia
Este proyecto está licenciado bajo la Licencia MIT - vea el archivo [LICENSE](LICENSE) para más detalles.
