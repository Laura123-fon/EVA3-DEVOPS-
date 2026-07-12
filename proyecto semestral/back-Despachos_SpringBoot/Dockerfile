# --- ETAPA 1: Compilación (Build con Maven estándar) ---
FROM maven:3.9-eclipse-temurin-17 AS build
WORKDIR /app

# El comodín "*/" extrae el pom.xml sin importar el nombre de la subcarpeta
COPY */pom.xml ./
RUN mvn dependency:go-offline -B

# Extrae la carpeta src sin importar el nombre de la subcarpeta
COPY */src ./src
RUN mvn clean package -DskipTests

# --- ETAPA 2: Ejecución ---
FROM eclipse-temurin:17-jre-alpine
WORKDIR /app

# Requisito Pauta: Usuario no-root para mayor seguridad
RUN addgroup -S spring && adduser -S spring -G spring
USER spring:spring

# Copiamos el .jar generado al contenedor final
COPY --from=build /app/target/*.jar app.jar

# PUERTO DE DOCUMENTACIÓN
EXPOSE 8091

CMD ["java", "-jar", "app.jar"]