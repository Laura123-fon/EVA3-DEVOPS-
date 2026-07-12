# Innovatech - Plataforma de Ventas y Despachos

Repositorio monorepo para la entrega final de DevOps. El proyecto implementa una solucion web para registrar ventas, generar ordenes de despacho y hacer seguimiento del estado de entrega. Incluye frontend, dos microservicios Spring Boot, contenedores Docker, manifiestos Kubernetes y pipeline CI/CD con GitHub Actions para despliegue en AWS.

## Integrantes

- Laura Fontecilla
- Ramiro Gomez

## Objetivo del proyecto

El sistema permite administrar el flujo entre ventas y despachos:

- Registrar y consultar ventas.
- Marcar ventas con despacho generado.
- Crear ordenes de despacho a partir de ventas pendientes.
- Consultar despachos existentes.
- Actualizar intentos de entrega y estado del despacho.
- Desplegar la aplicacion usando practicas DevOps: Docker, Kubernetes, ECR, EKS y GitHub Actions.

## Arquitectura general

```text
Usuario
  |
  v
Frontend React/Vite
  |
  +--> API Ventas Spring Boot ----+
  |                               |
  +--> API Despachos Spring Boot -+--> Base de datos MySQL/RDS

CI/CD:
GitHub Actions -> Docker build -> Amazon ECR -> Amazon EKS -> Kubernetes Services/HPA
```

## Contenido del repositorio

```text
.
|-- .github/workflows/deploy.yml
|-- README.md
`-- proyecto semestral/
    |-- back-Despachos_SpringBoot/
    |   |-- Dockerfile
    |   |-- Springboot-API-REST-DESPACHO/
    |   `-- k8s/
    |-- back-Ventas_SpringBoot/
    |   |-- Dockerfile
    |   |-- Springboot-API-REST/
    |   `-- k8s/
    `-- front_despacho/
        |-- Dockerfile
        |-- package.json
        |-- src/
        `-- k8s/
```

## Tecnologias utilizadas

- Java 17
- Spring Boot
- Spring Data JPA
- Maven Wrapper
- MySQL
- React 18
- Vite
- Tailwind CSS
- Axios
- Docker
- Kubernetes
- Horizontal Pod Autoscaler
- Amazon ECR
- Amazon EKS
- GitHub Actions

## Requisitos

Para ejecutar localmente:

- Java 17 o superior
- Node.js 18 o superior recomendado
- npm
- MySQL disponible
- Docker Desktop, si se desea construir contenedores
- kubectl, si se desea aplicar manifiestos Kubernetes

Para despliegue en AWS:

- Cuenta AWS Academy o AWS configurada
- AWS CLI
- Cluster EKS creado
- Repositorios ECR creados
- Credenciales AWS configuradas como secretos en GitHub

## Variables de entorno

Los backends leen la conexion a base de datos desde variables de entorno:

| Variable | Descripcion |
| --- | --- |
| `DB_ENDPOINT` | Host o endpoint de la base de datos MySQL/RDS |
| `DB_PORT` | Puerto de MySQL, normalmente `3306` |
| `DB_NAME` | Nombre de la base de datos |
| `DB_USERNAME` | Usuario de base de datos |
| `DB_PASSWORD` | Password de base de datos |

Ejemplo PowerShell para ejecucion local:

```powershell
$env:DB_ENDPOINT="localhost"
$env:DB_PORT="3306"
$env:DB_NAME="innovatech"
$env:DB_USERNAME="root"
$env:DB_PASSWORD="password"
```

Ejemplo Bash:

```bash
export DB_ENDPOINT=localhost
export DB_PORT=3306
export DB_NAME=innovatech
export DB_USERNAME=root
export DB_PASSWORD=password
```

## Ejecucion local

### Backend Ventas

```powershell
cd "proyecto semestral/back-Ventas_SpringBoot/Springboot-API-REST"
.\mvnw.cmd spring-boot:run
```

Endpoint base:

```text
http://localhost:8080/api/v1/ventas
```

### Backend Despachos

```powershell
cd "proyecto semestral/back-Despachos_SpringBoot/Springboot-API-REST-DESPACHO"
.\mvnw.cmd spring-boot:run
```

Endpoint base:

```text
http://localhost:8081/api/v1/despachos
```

### Frontend

```powershell
cd "proyecto semestral/front_despacho"
npm install
npm run dev
```

URL de desarrollo:

```text
http://localhost:5173
```

## Endpoints principales

### API Ventas

Base local:

```text
http://localhost:8080/api/v1/ventas
```

| Metodo | Ruta | Descripcion |
| --- | --- | --- |
| `GET` | `/api/v1/ventas` | Lista todas las ventas |
| `GET` | `/api/v1/ventas/{idVenta}` | Obtiene una venta por ID |
| `POST` | `/api/v1/ventas` | Crea una nueva venta |
| `PUT` | `/api/v1/ventas/{idVenta}` | Actualiza una venta |
| `DELETE` | `/api/v1/ventas/{idVenta}` | Elimina una venta |

Ejemplo JSON:

```json
{
  "direccionCompra": "Av. Siempre Viva 123",
  "valorCompra": 25000,
  "fechaCompra": "2026-06-24",
  "despachoGenerado": false
}
```

### API Despachos

Base local:

```text
http://localhost:8081/api/v1/despachos
```

| Metodo | Ruta | Descripcion |
| --- | --- | --- |
| `GET` | `/api/v1/despachos` | Lista todos los despachos |
| `GET` | `/api/v1/despachos/{idDespacho}` | Obtiene un despacho por ID |
| `POST` | `/api/v1/despachos` | Crea un nuevo despacho |
| `PUT` | `/api/v1/despachos/{idDespacho}` | Actualiza un despacho |
| `DELETE` | `/api/v1/despachos/{idDespacho}` | Elimina un despacho |

Ejemplo JSON:

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

## Documentacion Swagger/OpenAPI

Los backends incluyen configuracion OpenAPI. Una vez ejecutados, revisar:

```text
Ventas:    http://localhost:8080/swagger-ui/index.html
Despachos: http://localhost:8081/swagger-ui/index.html
```

## Pruebas y verificacion local

### Backend

```powershell
cd "proyecto semestral/back-Ventas_SpringBoot/Springboot-API-REST"
.\mvnw.cmd test
```

```powershell
cd "proyecto semestral/back-Despachos_SpringBoot/Springboot-API-REST-DESPACHO"
.\mvnw.cmd test
```

### Frontend

```powershell
cd "proyecto semestral/front_despacho"
npm run lint
npm run build
```

## Docker

Cada modulo incluye su propio `Dockerfile`.

### Imagen Frontend

```powershell
cd "proyecto semestral/front_despacho"
docker build -t innovatech-frontend:latest .
```

### Imagen Backend Ventas

```powershell
cd "proyecto semestral/back-Ventas_SpringBoot"
docker build -t innovatech-backend-ventas:latest .
```

### Imagen Backend Despachos

```powershell
cd "proyecto semestral/back-Despachos_SpringBoot"
docker build -t innovatech-backend-despachos:latest .
```

## Kubernetes

Cada servicio tiene manifiestos en su carpeta `k8s/`:

- `innovatech-deploy.yaml`: Deployment y Service.
- `innovatech-hpa.yaml`: autoscaling horizontal por CPU.
- `innovatech-secret.yaml`: secretos de base de datos, usado por los backends.

Aplicacion manual:

```powershell
kubectl apply -f "proyecto semestral/back-Ventas_SpringBoot/k8s/"
kubectl apply -f "proyecto semestral/back-Despachos_SpringBoot/k8s/"
kubectl apply -f "proyecto semestral/front_despacho/k8s/"
```

Comandos de verificacion:

```powershell
kubectl get deployments
kubectl get pods
kubectl get svc
kubectl get hpa
kubectl describe pod <nombre-del-pod>
kubectl logs <nombre-del-pod>
```

## Despliegue en AWS

El flujo de despliegue usado por el repositorio es:

1. Se hace push a la rama `deploy`.
2. GitHub Actions ejecuta el workflow `.github/workflows/deploy.yml`.
3. El pipeline configura credenciales AWS.
4. Se autentica contra Amazon ECR.
5. Construye las imagenes Docker.
6. Publica imagenes en ECR.
7. Actualiza kubeconfig para el cluster EKS `innovatech-eks`.
8. Aplica manifiestos Kubernetes.
9. Reinicia los deployments para tomar la nueva imagen.

Repositorios ECR esperados por el pipeline:

- `innovatech-frontend`
- `innovatech-backend-ventas`
- `innovatech-backend-despachos`

Region configurada:

```text
us-east-1
```

Secretos requeridos en GitHub Actions:

| Secreto | Uso |
| --- | --- |
| `AWS_ACCESS_KEY_ID` | Credencial AWS |
| `AWS_SECRET_ACCESS_KEY` | Credencial AWS |
| `AWS_SESSION_TOKEN` | Token de sesion AWS Academy |
| `DB_ENDPOINT` | Endpoint MySQL/RDS |
| `DB_PORT` | Puerto MySQL |
| `DB_NAME` | Base de datos |
| `DB_USERNAME` | Usuario de BD |
| `DB_PASSWORD` | Password de BD |

## Evidencias recomendadas para la presentacion

Para defender la entrega, se recomienda mostrar:

- Repositorio con estructura monorepo.
- Workflow de GitHub Actions ejecutado correctamente.
- Imagenes publicadas en Amazon ECR.
- Cluster EKS activo.
- Pods en estado `Running`.
- Services creados para frontend y backends.
- HPA creado para cada servicio.
- Aplicacion frontend funcionando.
- Pruebas de endpoints con Postman, navegador o Swagger.

## Consideraciones importantes

- El backend de Ventas usa puerto local `8080`.
- El backend de Despachos usa puerto local `8081`.
- Los Dockerfile de backend exponen puertos documentales `8091` y `8092`, pero la aplicacion toma el puerto desde Spring Boot. Para ejecucion final se debe validar que `EXPOSE`, `server.port` y el `containerPort` de Kubernetes coincidan.
- En los manifiestos Kubernetes, algunos HPA referencian nombres de deployment que deben coincidir exactamente con los deployments creados.
- El frontend contiene URLs de API hardcodeadas en componentes React. Para un entorno final se recomienda moverlas a variables de entorno de Vite, por ejemplo `VITE_API_VENTAS_URL` y `VITE_API_DESPACHOS_URL`.
- En `back-Ventas_SpringBoot` existe una clase llamada `OpenApiConfing`; el nombre parece tener un typo y podria normalizarse a `OpenApiConfig`.

## Estado actual

El repositorio contiene:

- Frontend React/Vite para gestion de despachos.
- Backend Ventas con CRUD.
- Backend Despachos con CRUD.
- Dockerfile por servicio.
- Manifiestos Kubernetes por servicio.
- Pipeline CI/CD para rama `deploy`.
- Documentacion base para ejecucion, despliegue y verificacion.

## Contribucion

Flujo sugerido:

1. Crear rama `feature/nombre-cambio` o `fix/nombre-cambio`.
2. Realizar cambios.
3. Probar localmente.
4. Crear Pull Request.
5. Hacer merge hacia `deploy` cuando corresponda desplegar.

## Licencia

Proyecto academico desarrollado para evaluacion final transversal.
