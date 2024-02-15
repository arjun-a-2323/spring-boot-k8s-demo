FROM maven:3.9.6-eclipse-temurin-21-alpine AS build
WORKDIR /app

COPY pom.xml .

# Download dependencies
RUN mvn dependency:go-offline

# Copy source code and build the application
COPY src src
RUN mvn package -DskipTests && \
    mkdir -p target/dependency && (cd target/dependency; jar -xf ../*.jar)

FROM gcr.io/distroless/java21-debian12
VOLUME /tmp
ARG DEPENDENCY=/app/target/dependency
COPY --from=build ${DEPENDENCY}/BOOT-INF/lib /app/lib
COPY --from=build ${DEPENDENCY}/META-INF /app/META-INF
COPY --from=build ${DEPENDENCY}/BOOT-INF/classes /app
USER nonroot:nonroot
ENTRYPOINT ["java","-cp","app:app/lib/*","com.example.demo.Main", "--spring.config.location=file:/config/"]