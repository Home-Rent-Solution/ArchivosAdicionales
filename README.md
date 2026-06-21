# Home Rent Solution - Archivos Adicionales

Repositorio central de apoyo para el proyecto academico Home Rent Solution, desarrollado con arquitectura de microservicios en Spring Boot.

Este repositorio contiene los archivos generales de despliegue local con Docker, configuracion de variables de entorno y script inicial de creacion de bases de datos MySQL.

## Integrantes

- Noemi Paillallao
- Victor Urra
- Catalina Barrios

## Descripcion del proyecto

Home Rent Solution es una solucion orientada a la gestion de arriendos de propiedades.

El sistema se divide en microservicios independientes, cada uno con una responsabilidad especifica dentro del negocio.

La arquitectura considera:

- Microservicios con Spring Boot.
- API Gateway para centralizar el acceso a las rutas.
- Eureka Server para descubrimiento de servicios.
- MySQL como motor de base de datos.
- RabbitMQ como servicio de mensajeria.
- Docker Compose para levantar el entorno completo de forma local.

## Archivos incluidos en este repositorio

- docker-compose.yml
- .env.example
- docker/mysql/init/01-create-databases.sql
- comando de verificacion compose.txt

## Descripcion de archivos

| Archivo | Descripcion |
|---|---|
| docker-compose.yml | Orquesta MySQL, RabbitMQ, Eureka, API Gateway y los microservicios del sistema. |
| .env.example | Archivo de ejemplo para configurar variables de entorno. |
| docker/mysql/init/01-create-databases.sql | Script SQL que crea las bases de datos utilizadas por los microservicios. |
| comando de verificacion compose.txt | Guia de comandos para validar pruebas, cobertura, Docker Compose y logs. |

## Microservicios del sistema

| Microservicio | Puerto | Responsabilidad principal |
|---|---:|---|
| MS-Comentarios | 8080 | Gestion de comentarios de los usuarios. |
| MS-Propiedades | 8081 | Gestion de propiedades disponibles para arriendo. |
| MS-Anfitriones | 8082 | Gestion de anfitriones o duenos de propiedades. |
| MS-Inquilinos | 8083 | Gestion de inquilinos. |
| MS-Reservas | 8084 | Gestion de reservas de propiedades. |
| MS-Pagos | 8085 | Gestion de pagos asociados a reservas. |
| MS-Limpieza | 8086 | Gestion de servicios de limpieza. |
| MS-Mensajeria | 8087 | Gestion de mensajes entre usuarios. |
| MS-Precios | 8088 | Gestion de precios por temporada y propiedad. |
| MS-Seguros | 8089 | Gestion de seguros asociados a reservas. |
| API-Gateway | 8090 | Entrada central para consumir los microservicios. |
| Eureka | 8761 | Registro y descubrimiento de servicios. |

## Rutas principales mediante API Gateway

El API Gateway se expone en:

```text
http://localhost:8090
```

| Servicio | Ruta por Gateway |
|---|---|
| Comentarios | http://localhost:8090/api/v1/comentarios |
| Propiedades | http://localhost:8090/api/v1/propiedades |
| Anfitriones | http://localhost:8090/api/v1/anfitriones |
| Inquilinos | http://localhost:8090/api/v1/inquilinos |
| Reservas | http://localhost:8090/api/v1/reservas |
| Pagos | http://localhost:8090/api/v1/pagos |
| Pagos V2 | http://localhost:8090/api/v2/pagos |
| Limpieza | http://localhost:8090/api/v1/limpiezas |
| Limpieza V2 | http://localhost:8090/api/v2/limpiezas |
| Mensajeria | http://localhost:8090/api/v1/mensajes |
| Precios | http://localhost:8090/api/v1/precios |
| Seguros | http://localhost:8090/api/v1/seguros |
| Seguros V2 | http://localhost:8090/api/v2/seguros |

## Servicios de infraestructura

| Servicio | URL local |
|---|---|
| Eureka Server | http://localhost:8761 |
| API Gateway | http://localhost:8090 |
| RabbitMQ Management | http://localhost:15672 |
| MySQL | localhost:3306 |

## Ejecucion local con Docker

Este repositorio contiene los archivos centrales de despliegue.

Para ejecutar correctamente el entorno, el archivo docker-compose.yml debe estar ubicado al mismo nivel que las carpetas de los microservicios.

Estructura esperada:

```text
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
```

## Paso 1: Crear archivo .env

Copiar el archivo de ejemplo:

```powershell
Copy-Item .env.example .env
```

Contenido esperado:

```text
MYSQL_ROOT_PASSWORD=Duoc.2026
```

## Paso 2: Levantar el entorno

Ejecutar:

```powershell
docker compose up --build
```

Para ejecutarlo en segundo plano:

```powershell
docker compose up --build -d
```

## Paso 3: Detener el entorno

Ejecutar:

```powershell
docker compose down
```

## Bases de datos creadas automaticamente

El script docker/mysql/init/01-create-databases.sql crea las siguientes bases de datos:

- db_comentarios_dev
- db_propiedades_dev
- db_anfitriones_dev
- db_inquilinos_dev
- db_reservas
- db_pagos
- db_limpieza
- db_mensajeria
- db_precios
- db_seguros

## Comunicacion entre servicios

Los microservicios se comunican mediante nombres internos de Docker, por ejemplo:

```text
http://ms-reservas:8084
http://ms-pagos:8085
http://ms-propiedades:8081
```

Esto evita el uso de localhost dentro de contenedores y permite que los servicios se encuentren correctamente al ejecutarse con Docker Compose.

## Pruebas

Cada microservicio mantiene sus propias pruebas unitarias y de integracion.

Para ejecutar pruebas en un microservicio especifico:

```powershell
mvn test
```

## Cobertura JaCoCo

Todos los microservicios cuentan con pruebas unitarias y cobertura JaCoCo igual o superior al 80%, cumpliendo con el minimo solicitado en la evaluacion.

| Microservicio | Cobertura |
|---|---:|
| MS-Anfitriones | 84% |
| MS-Comentarios | 84% |
| MS-Propiedades | 87% |
| MS-Inquilinos | 88,46% |
| MS-Limpieza | 80% |
| MS-Mensajeria | 82% |
| MS-Pagos | 83,98% |
| MS-Precios | 86% |
| MS-Reservas | 81,60% |
| MS-Seguros | 91% |

## Documentacion Swagger

Swagger se encuentra habilitado y centralizado mediante API Gateway.

Swagger central:

```text
http://localhost:8090/swagger-ui.html
```

Desde esta interfaz se puede consultar la documentacion de los endpoints expuestos por los microservicios.

## Gestion colaborativa

La organizacion y seguimiento de tareas del equipo se realizo mediante ClickUp, donde se registraron tareas, responsables y avance del proyecto.

La evidencia del tablero fue subida como parte de la entrega del equipo.

## Validacion tecnica final

Antes de la entrega se valido el funcionamiento del ecosistema con los siguientes comandos:

```powershell
docker compose config
docker compose up --build -d
docker compose ps
```

Tambien se valido:

- Eureka Server operativo en `http://localhost:8761`.
- API Gateway operativo en `http://localhost:8090`.
- Swagger centralizado operativo en `http://localhost:8090/swagger-ui.html`.
- Microservicios registrados correctamente en Eureka.
- Rutas principales del Gateway respondiendo correctamente.
- Logs recientes sin errores criticos.

## Comandos utiles de verificacion

Validar Eureka:

```powershell
Invoke-WebRequest http://localhost:8761 -UseBasicParsing | Select-Object StatusCode
```

Validar Swagger Gateway:

```powershell
Invoke-WebRequest http://localhost:8090/swagger-ui.html -UseBasicParsing | Select-Object StatusCode
```

Validar servicios registrados en Eureka:

```powershell
[xml]$apps = (Invoke-WebRequest http://localhost:8761/eureka/apps -UseBasicParsing).Content
$apps.applications.application.name
```

Validar logs recientes sin errores:

```powershell
docker compose logs --since 2m | Select-String -Pattern "ERROR|Exception|Failed|Connection refused"
```

## Observaciones

- El archivo .env.example no contiene credenciales reales de produccion.
- El entorno Docker esta orientado a ejecucion local academica.
- Cada microservicio mantiene su propio repositorio y responsabilidad.
- El API Gateway centraliza el acceso externo a los servicios.
- Eureka permite registrar y descubrir los microservicios activos.
