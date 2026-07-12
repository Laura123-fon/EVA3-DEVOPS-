# Backend Ventas - Innovatech

Microservicio Spring Boot encargado de administrar las ventas del sistema Innovatech. Expone una API REST CRUD para crear, listar, actualizar y eliminar ventas.

## Tecnologias

- Java 17
- Spring Boot
- Spring Web
- Spring Data JPA
- MySQL
- Maven
- OpenAPI/Swagger
- Docker
- Kubernetes

## Estructura

```text
back-Ventas_SpringBoot/
|-- Dockerfile
|-- Springboot-API-REST/
|   |-- pom.xml
|   `-- src/
`-- k8s/
    |-- innovatech-deploy.yaml
    |-- innovatech-hpa.yaml
    `-- innovatech-secret.yaml
```

## Variables de entorno

| Variable | Descripcion |
| --- | --- |
| `DB_ENDPOINT` | Endpoint o host de MySQL |
| `DB_PORT` | Puerto de MySQL |
| `DB_NAME` | Nombre de la base de datos |
| `DB_USERNAME` | Usuario de base de datos |
| `DB_PASSWORD` | Password de base de datos |

## Ejecucion local

```powershell
cd "Springboot-API-REST"
.\mvnw.cmd spring-boot:run
```

URL base:

```text
http://localhost:8080/api/v1/ventas
```

## Endpoints

| Metodo | Ruta | Descripcion |
| --- | --- | --- |
| `GET` | `/api/v1/ventas` | Lista todas las ventas |
| `GET` | `/api/v1/ventas/{idVenta}` | Obtiene una venta |
| `POST` | `/api/v1/ventas` | Crea una venta |
| `PUT` | `/api/v1/ventas/{idVenta}` | Actualiza una venta |
| `DELETE` | `/api/v1/ventas/{idVenta}` | Elimina una venta |

## Ejemplo JSON

```json
{
  "direccionCompra": "Av. Siempre Viva 123",
  "valorCompra": 25000,
  "fechaCompra": "2026-06-24",
  "despachoGenerado": false
}
```

## Pruebas

```powershell
cd "Springboot-API-REST"
.\mvnw.cmd test
```

## Docker

```powershell
docker build -t innovatech-backend-ventas:latest .
```

## Kubernetes

```powershell
kubectl apply -f k8s/
kubectl get pods
kubectl get svc
kubectl get hpa
```

## Swagger

Con el servicio activo:

```text
http://localhost:8080/swagger-ui/index.html
```
