# Używamy obrazu Maven z JDK 17 jako obrazu bazowego
FROM maven:3.8.4-openjdk-17-slim AS builder

# Ustawiamy katalog roboczy w kontenerze
WORKDIR /app

# Kopiujemy plik pom.xml i pobieramy zależności (aby nie kopiować wszystkich plików)
COPY pom.xml .

# Pobieramy zależności Maven (będziemy to robić w osobnym kroku, by nie powtarzać pobierania przy każdej zmianie w kodzie)
RUN mvn dependency:go-offline

# Kopiujemy cały kod źródłowy projektu
COPY src ./src

# Budujemy aplikację (tworzymy plik JAR)
RUN mvn clean package -DskipTests

# Drugi etap: używamy obrazu OpenJDK do uruchomienia aplikacji
FROM openjdk:17-slim

# Ustawiamy katalog roboczy w kontenerze
WORKDIR /app

# Kopiujemy plik JAR do kontenera
COPY --from=builder /app/target/*.jar app.jar

# Otwieramy port 8080 (domyślny port Spring Boot)
EXPOSE 8080

# Uruchamiamy aplikację Spring Boot
ENTRYPOINT ["java", "-jar", "app.jar"]