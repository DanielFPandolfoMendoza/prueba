# Etapa 1: Construcción
FROM openjdk:21-jdk-slim AS BUILD

# Establecer el directorio de trabajo
WORKDIR /app

# Instalar Maven 4 manualmente
RUN apt-get update && \
    apt-get install -y wget unzip && \
    wget https://downloads.apache.org/maven/maven-4/4.0.0-alpha-7/binaries/apache-maven-4.0.0-alpha-7-bin.zip && \
    unzip apache-maven-4.0.0-alpha-7-bin.zip -d /opt && \
    ln -s /opt/apache-maven-4.0.0-alpha-7/bin/mvn /usr/bin/mvn && \
    rm -rf apache-maven-4.0.0-alpha-7-bin.zip

# Copiar los archivos del proyecto al contenedor
COPY . .

# Construir el JAR con Maven
RUN mvn clean package -DskipTests

# Etapa 2: Imagen para ejecución
FROM openjdk:21-jdk-slim

# Establecer el directorio de trabajo
WORKDIR /app

# Copiar el JAR generado desde la etapa de construcción
COPY --from=BUILD /app/target/*.jar app.jar

# Exponer el puerto 8080
EXPOSE 8080

# Comando para ejecutar la aplicación
ENTRYPOINT ["java", "-jar", "app.jar"]

