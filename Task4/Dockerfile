# Стадия сборки: используем образ с Maven и JDK 17
FROM maven:3.8-openjdk-17 AS builder

# Устанавливаем рабочую директорию внутри контейнера
WORKDIR /app

# Копируем pom.xml для загрузки зависимостей
COPY pom.xml .

# Загружаем зависимости проекта (кэшируется, если pom.xml не менялся)
RUN mvn dependency:go-offline

# Копируем исходный код проекта
COPY src ./src

# Собираем jar-файл, пропуская тесты
RUN mvn clean package -DskipTests

# Стадия запуска: используем лёгкий образ только с JRE
FROM eclipse-temurin:17-jre-alpine

# Устанавливаем рабочую директорию
WORKDIR /app

# Копируем собранный jar из стадии сборки
COPY --from=builder /app/target/*.jar app.jar

# Открываем порт 8080 для доступа к приложению
EXPOSE 8080

# Запускаем приложение при старте контейнера
ENTRYPOINT ["java", "-jar", "app.jar"]
