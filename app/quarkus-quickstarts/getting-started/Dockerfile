# Stage 1: build (compilação dentro do container)
FROM maven:3.9-eclipse-temurin-17 AS build
WORKDIR /workspace
COPY pom.xml .
COPY src ./src
# Compila sem rodar testes para agilizar (já testamos no passo anterior)
RUN mvn -e -DskipTests package

# ==== Stage 2: runtime (imagem final leve, base exigida no edital) ====
FROM eclipse-temurin:17-jre
WORKDIR /app
# Copia somente os artefatos necessários do build (sem Maven, sem fontes)
COPY --from=build /workspace/target/quarkus-app/ /app/
EXPOSE 8080
# Inicia a aplicação Quarkus empacotada em fast-jar
ENTRYPOINT ["java","-jar","/app/quarkus-run.jar"]
