# Этап сборки
FROM node:18-alpine AS builder

# Переменная для сборки
ENV NODE_OPTIONS=--openssl-legacy-provider

# Пользователь и группа
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

# Рабочая директория
WORKDIR /app

# Копирование файлов зависимостей
COPY package.json package-lock.json ./

# Установка зависимостей
RUN npm ci --no-audit --no-fund

# Копирование остальных файлов
COPY . .

# Сборка
RUN npm run build && \
    chown -R appuser:appgroup /app/dist

# Этап запуска приложения
FROM nginx:1.25-alpine

# Пользователь и группа
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

# Директория приложения
RUN mkdir -p /usr/share/nginx/html/momo-store

# Копирование собранного приложения из этапа сборки
COPY --from=builder --chown=appuser:appgroup /app/dist/ /usr/share/nginx/html/momo-store/

# Копирование конфигурации для nginx
RUN mkdir -p /etc/nginx/conf.d
COPY --chown=appuser:appgroup nginx.conf /etc/nginx/conf.d/default.conf

# Установка правильных прав доступа для директорий
RUN chown -R appuser:appgroup /var/cache/nginx && \
    chown -R appuser:appgroup /var/log/nginx && \
    chown -R appuser:appgroup /etc/nginx/conf.d && \
    touch /var/run/nginx.pid && \
    chown -R appuser:appgroup /var/run/nginx.pid && \
    chmod -R 755 /usr/share/nginx/html

# Переключение на созданного непривилегированного пользователя
USER appuser

# Открытие порта
EXPOSE 80

# Проверка работоспособности приложения
HEALTHCHECK --interval=30s --timeout=5s --start-period=5s --retries=3 \
CMD curl -f http://localhost/momo-store/ || exit 1

# Запуск nginx
CMD ["nginx", "-g", "daemon off;"]