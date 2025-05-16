# Этап сборки
FROM golang:1.22.7-alpine AS builder

# Рабочая директория
WORKDIR /app

# Копирование файлов зависимостей
COPY go.mod go.sum ./

# Загрузка зависимостей
RUN go mod download

# Копирование остальных файлов
COPY . .

# Сборка приложения
RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -ldflags="-w -s" -o /go/bin/app ./cmd/api

# Этап запуска приложения
FROM alpine:3.18

# Установка сертификатов CA для HTTPS запросов
RUN apk --no-cache add ca-certificates && \
    update-ca-certificates

# Создаем непривилегированного пользователя
RUN adduser -D -g '' appuser

# Создаем директорию для приложения
WORKDIR /app

# Копирование исполняемого файла из builder
COPY --from=builder /go/bin/app /app/momo-store

# Настройка прав доступа
RUN chmod 500 /app/momo-store && \
    chown -R appuser:appuser /app

# Переключение на созданного непривилегированного пользователя
USER appuser

# Порт
EXPOSE 8081

# Запуск приложения
CMD ["/app/momo-store"]