# Проект «Cloud Services Engineer Docker Project Sem2»

**Краткое описание**  
Учебный проект по курсу «Cloud Services Engineer». Он демонстрирует сборку и оркестрацию микросервисов с помощью Docker Compose. В составе проекта:

- **backend** — микросервис на Go, реализующий REST-API.
- **frontend** — приложение на Vue.js + TypeScript.

---

## Содержание

1. [Технологии](#технологии)
2. [Требования](#требования)  
3. [Установка и запуск](#установка-и-запуск)
4. [Оптимизация](#оптимизация)
---

## Технологии

- **Go** (backend)
- **Vue.js + TypeScript + Nginx** (frontend)
- **Docker** & **Docker Compose**

---

## Требования

- **Docker >= 20.10**
- **Docker Compose >= 1.29**
- **Git**

---

## Установка и запуск

1. Клонировать репозиторий
```bash
git clone https://github.com/ClarkeFeed/cloud-services-engineer-docker-project-sem2.git
cd cloud-services-engineer-docker-project-sem2
```

2. Запустить проект со сборкой
```bash
docker-compose up --build -d
```

3. Проверить запущенные контейнеры
```bash
docker ps
```

После успешного запуска должны быть доступны:
- Frontend: http://localhost(:80)
- Backend: http://localhost:8081

## Оптимизация

Для уменьшения веса образов в сборке использовались следующие приёмы:
1. alpine образы;
2. флаг `CGO_ENABLED=0` для backend;
3. флаги `--no-audit --no-fund` для frontend.

Для nginx был настроен gzip и Cache-Control для кеширования и уменьшения размера передаваемых файлов.