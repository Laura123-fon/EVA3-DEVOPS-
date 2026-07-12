# Backend Despachos - Innovatech

Microservicio Spring Boot encargado de administrar las ordenes de despacho del sistema Innovatech. Expone una API REST CRUD para crear, listar, actualizar y eliminar despachos.

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
back-Despachos_SpringBoot/
|-- Dockerfile
|-- Springboot-API-REST-DESPACHO/
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
cd "Springboot-API-REST-DESPACHO"
.\mvnw.cmd spring-boot:run
```

URL base:

```text
http://localhost:8081/api/v1/despachos
```

## Endpoints

| Metodo | Ruta | Descripcion |
| --- | --- | --- |
| `GET` | `/api/v1/despachos` | Lista todos los despachos |
| `GET` | `/api/v1/despachos/{idDespacho}` | Obtiene un despacho |
| `POST` | `/api/v1/despachos` | Crea un despacho |
| `PUT` | `/api/v1/despachos/{idDespacho}` | Actualiza un despacho |
| `DELETE` | `/api/v1/despachos/{idDespacho}` | Elimina un despacho |

## Ejemplo JSON

```json
{
  "fechaDespacho": "2026-06-25",
  "patenteCamion": "ABCD12",
  "intento": 1,
  "idCompra": 1,
  "direccionCompra": "Av. Siempre Viva 123",
  "valorCompra": 25000,
  "despachado": false
}
```

## Pruebas

```powershell
cd "Springboot-API-REST-DESPACHO"
.\mvnw.cmd test
```

## Docker

```powershell
docker build -t innovatech-backend-despachos:latest .
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
http://localhost:8081/swagger-ui/index.html
```
