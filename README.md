Home Rent Solution - Archivos Adicionales



Repositorio central de apoyo para el proyecto académico Home Rent Solution, desarrollado con arquitectura de microservicios en Spring Boot.



Este repositorio contiene los archivos generales de despliegue local con Docker, configuración de variables de entorno y script inicial de creación de bases de datos MySQL.



Integrantes

Noemi Paillallao

Victor Urra

Catalina Barrios

Descripción del proyecto



Home Rent Solution es una solución orientada a la gestión de arriendos de propiedades.

El sistema se divide en microservicios independientes, cada uno con una responsabilidad específica dentro del negocio.



La arquitectura considera:



Microservicios con Spring Boot.

API Gateway para centralizar el acceso a las rutas.

Eureka Server para descubrimiento de servicios.

MySQL como motor de base de datos.

RabbitMQ como servicio de mensajería.

Docker Compose para levantar el entorno completo de forma local.

Archivos incluidos en este repositorio

docker-compose.yml

.env.example

docker/mysql/init/01-create-databases.sql

Descripción de archivos

Archivo	Descripción

docker-compose.yml	Orquesta MySQL, RabbitMQ, Eureka, API Gateway y los microservicios del sistema.

.env.example	Archivo de ejemplo para configurar variables de entorno.

docker/mysql/init/01-create-databases.sql	Script SQL que crea las bases de datos utilizadas por los microservicios.

Microservicios del sistema

Microservicio	Puerto	Responsabilidad principal

MS-Comentarios	8080	Gestión de comentarios de los usuarios.

MS-Propiedades	8081	Gestión de propiedades disponibles para arriendo.

MS-Anfitriones	8082	Gestión de anfitriones o dueños de propiedades.

MS-Inquilinos	8083	Gestión de inquilinos.

MS-Reservas	8084	Gestión de reservas de propiedades.

MS-Pagos	8085	Gestión de pagos asociados a reservas.

MS-Limpieza	8086	Gestión de servicios de limpieza.

MS-Mensajeria	8087	Gestión de mensajes entre usuarios.

MS-Precios	8088	Gestión de precios por temporada y propiedad.

MS-Seguros	8089	Gestión de seguros asociados a reservas.

API-Gateway	8090	Entrada central para consumir los microservicios.

Eureka	8761	Registro y descubrimiento de servicios.

Rutas principales mediante API Gateway



El API Gateway se expone en:



http://localhost:8090



Rutas principales:



Servicio	Ruta por Gateway

Comentarios	http://localhost:8090/api/v1/comentarios

Propiedades	http://localhost:8090/api/v1/propiedades

Anfitriones	http://localhost:8090/api/v1/anfitriones

Inquilinos	http://localhost:8090/api/v1/inquilinos

Reservas	http://localhost:8090/api/v1/reservas

Pagos	http://localhost:8090/api/v1/pagos

Limpieza	http://localhost:8090/api/v1/limpieza

Mensajería	http://localhost:8090/api/v1/mensajes

Precios	http://localhost:8090/api/v1/precios

Seguros	http://localhost:8090/api/v1/seguros

Servicios de infraestructura

Servicio	URL local

Eureka Server	http://localhost:8761

API Gateway	http://localhost:8090

RabbitMQ Management	http://localhost:15672

MySQL	localhost:3306

Ejecución local con Docker



Este repositorio contiene los archivos centrales de despliegue.

Para ejecutar correctamente el entorno, el archivo docker-compose.yml debe estar ubicado al mismo nivel que las carpetas de los microservicios.



Estructura esperada:



Home-Rent-Solution/

├── docker-compose.yml

├── .env

├── docker/

│   └── mysql/

│       └── init/

│           └── 01-create-databases.sql

├── Eureka/

├── API-Gateway/

├── MS-Comentarios/

├── MS-Propiedades/

├── MS-Anfitriones/

├── MS-Inquilinos/

├── MS-Reservas/

├── MS-Pagos/

├── MS-Limpieza/

├── MS-Mensajeria/

├── MS-Precios/

└── MS-Seguros/

Paso 1: Crear archivo .env



Copiar el archivo de ejemplo:



Copy-Item .env.example .env



Contenido esperado:



MYSQL\_ROOT\_PASSWORD=Duoc.2026

Paso 2: Levantar el entorno



Ejecutar:



docker compose up --build

Paso 3: Detener el entorno



Ejecutar:



docker compose down

Bases de datos creadas automáticamente



El script docker/mysql/init/01-create-databases.sql crea las siguientes bases de datos:



db\_comentarios\_dev

db\_propiedades\_dev

db\_anfitriones\_dev

db\_inquilinos\_dev

db\_reservas

db\_pagos

db\_limpieza

db\_mensajeria

db\_precios

db\_seguros

Comunicación entre servicios



Los microservicios se comunican mediante nombres internos de Docker, por ejemplo:



http://ms-reservas:8084

http://ms-pagos:8085

http://ms-propiedades:8081



Esto evita el uso de localhost dentro de contenedores y permite que los servicios se encuentren correctamente al ejecutarse con Docker Compose.



Pruebas



Cada microservicio mantiene sus propias pruebas unitarias y de integración.



Para ejecutar pruebas en un microservicio específico:



mvn test

Documentación Swagger



En esta versión del proyecto se prioriza la exposición de endpoints mediante API Gateway y pruebas con Postman.



Si Swagger se encuentra habilitado en algún microservicio, la ruta local esperada sería:



http://localhost:PUERTO/swagger-ui/index.html



Ejemplo:



http://localhost:8087/swagger-ui/index.html

Observaciones

El archivo .env.example no contiene credenciales reales de producción.

El entorno Docker está orientado a ejecución local académica.

Cada microservicio mantiene su propio repositorio y responsabilidad.

El API Gateway centraliza el acceso externo a los servicios.

Eureka permite registrar y descubrir los microservicios activos.

