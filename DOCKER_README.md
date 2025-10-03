# Docker Setup для Store Projects

Этот проект содержит два отдельных Next.js приложения:
- **store-admin** - административная панель (порт 3000)
- **store-client** - клиентское приложение (порт 3001)

## Требования

- Docker
- Docker Compose

## Быстрый старт

1. **Скопируйте файл с переменными окружения:**
   ```bash
   cp env.example .env
   ```

2. **Отредактируйте .env файл** с вашими настройками:
   - DATABASE_URL - подключение к вашей базе данных
   - AWS настройки для S3
   - Telegram Bot настройки
   - NextAuth секреты

3. **Запустите проекты:**
   ```bash
   docker-compose up --build
   ```

4. **Доступ к приложениям:**
   - Админ панель: http://localhost:3000
   - Клиентское приложение: http://localhost:3001

## Команды Docker

### Запуск в фоновом режиме
```bash
docker-compose up -d --build
```

### Остановка
```bash
docker-compose down
```

### Просмотр логов
```bash
docker-compose logs -f
```

### Пересборка без кэша
```bash
docker-compose build --no-cache
```

### Остановка и удаление контейнеров
```bash
docker-compose down -v
```

## Структура проекта

```
store/
├── docker-compose.yml          # Docker Compose конфигурация
├── .env                        # Переменные окружения (создать из env.example)
├── env.example                 # Пример переменных окружения
├── store-admin/                # Административная панель
│   ├── Dockerfile
│   ├── .dockerignore
│   └── ...
└── store-client/               # Клиентское приложение
    ├── Dockerfile
    ├── .dockerignore
    └── ...
```

## Переменные окружения

Основные переменные, которые нужно настроить в .env:

- `DATABASE_URL` - строка подключения к PostgreSQL
- `NEXTAUTH_SECRET` - секретный ключ для NextAuth
- `AWS_ACCESS_KEY_ID` - AWS ключ доступа
- `AWS_SECRET_ACCESS_KEY` - AWS секретный ключ
- `AWS_S3_BUCKET` - название S3 bucket
- `TELEGRAM_BOT_TOKEN` - токен Telegram бота
- `TELEGRAM_CHAT_ID` - ID чата для уведомлений

## Порты

- **3000** - store-admin (административная панель)
- **3001** - store-client (клиентское приложение)

## Примечания

- База данных должна быть доступна извне Docker контейнеров
- Убедитесь, что все необходимые переменные окружения настроены
- Для production использования рекомендуется использовать reverse proxy (nginx)
