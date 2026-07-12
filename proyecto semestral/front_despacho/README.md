# Frontend Despachos - Innovatech

Aplicacion web desarrollada con React y Vite para administrar el flujo de despachos de Innovatech. Permite revisar ventas pendientes, generar ordenes de despacho y actualizar el estado de entrega.

## Tecnologias

- React 18
- Vite
- Tailwind CSS
- Axios
- React Router DOM
- React Hook Form
- SweetAlert2
- Docker
- Nginx para imagen productiva

## Requisitos

- Node.js 18 o superior recomendado
- npm
- Backend Ventas disponible
- Backend Despachos disponible

## Instalacion

```powershell
npm install
```

## Ejecucion en desarrollo

```powershell
npm run dev
```

URL por defecto:

```text
http://localhost:5173
```

## Scripts disponibles

| Comando | Descripcion |
| --- | --- |
| `npm run dev` | Levanta servidor local Vite |
| `npm run build` | Genera build productiva |
| `npm run lint` | Ejecuta validacion ESLint |
| `npm run preview` | Sirve una previsualizacion del build |

## Funcionalidades principales

- Visualizacion de ventas pendientes de despacho.
- Generacion de una orden de despacho desde una venta.
- Actualizacion de la venta para marcar `despachoGenerado`.
- Listado de despachos.
- Cierre o actualizacion de despacho.
- Modales y alertas de confirmacion.

## Integracion con APIs

El frontend consume:

- API Ventas: `/api/v1/ventas`
- API Despachos: `/api/v1/despachos`

Actualmente existen URLs hardcodeadas en algunos componentes. Para despliegues en distintos ambientes se recomienda migrar estas rutas a variables de entorno Vite:

```text
VITE_API_VENTAS_URL=http://localhost:8080/api/v1/ventas
VITE_API_DESPACHOS_URL=http://localhost:8081/api/v1/despachos
```

## Docker

Construir imagen:

```powershell
docker build -t innovatech-frontend:latest .
```

El Dockerfile usa una etapa de build con Node y una etapa final con Nginx.

## Kubernetes

Los manifiestos estan en:

```text
k8s/
```

Incluyen:

- Deployment del frontend.
- Service para exponer la aplicacion.
- HPA para autoscaling.

Aplicar manifiestos:

```powershell
kubectl apply -f k8s/
```

Verificar:

```powershell
kubectl get pods
kubectl get svc
kubectl get hpa
```

## Build de produccion

```powershell
npm run build
```

La salida queda en:

```text
dist/
```

## Notas

- La ruta principal de la aplicacion es `/`.
- El componente principal se encuentra en `src/componentes/CrudAdmin.jsx`.
- Las rutas se definen en `src/Routes/AppRoutes.jsx`.
- Antes de desplegar en EKS, validar que las URLs de API apunten a los services correctos del cluster.
