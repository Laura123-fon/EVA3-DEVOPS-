# --- ETAPA 1: Compilación (Build con Maven estándar) ---
FROM maven:3.9-eclipse-temurin-17 AS build
WORKDIR /app

# Corrección: Referencia directa. El comando 'docker build' debe ejecutarse dentro de la carpeta del microservicio.
COPY pom.xml ./
RUN mvn dependency:go-offline -B

COPY src ./src
RUN mvn clean package -DskipTests

# --- ETAPA 2: Ejecución ---
FROM eclipse-temurin:17-jre-alpine
WORKDIR /app

# Requisito Pauta: Usuario no-root para mayor seguridad
RUN addgroup -S spring && adduser -S spring -G spring
USER spring:spring

# Copiamos el .jar generado al contenedor final
COPY --from=build /app/target/*.jar app.jar

# Corrección: Estandarización al puerto 8080 para coincidir con la red de Kubernetes
EXPOSE 8080

CMD ["java", "-jar", "app.jar"]