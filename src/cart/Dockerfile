# ==========================
# Build Stage!
# =========================
FROM public.ecr.aws/amazonlinux/amazonlinux:2023 AS build

RUN dnf install -y \
    maven \
    java-21-amazon-corretto-headless \
    tar \
    gzip \
    && dnf clean all

WORKDIR /build

COPY .mvn .mvn
COPY mvnw .
COPY pom.xml .

RUN chmod +x mvnw \
    && ./mvnw dependency:go-offline -B

COPY src src

RUN ./mvnw clean package -DskipTests \
    && mv target/carts-0.0.1-SNAPSHOT.jar app.jar


# =========================
# Runtime Stage
# =========================
FROM public.ecr.aws/amazonlinux/amazonlinux:2023

RUN dnf install -y \
    java-21-amazon-corretto-headless \
    && dnf clean all

WORKDIR /app

COPY --from=build /build/app.jar app.jar

ENV SPRING_PROFILES_ACTIVE=prod

EXPOSE 8080

ENTRYPOINT ["java", "-jar", "app.jar"]