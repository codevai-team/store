# Развертывание на Timeweb

## Настройка доменов

Проект настроен для работы с поддоменами:
- **Основной сайт**: `example.com` (замените на ваш домен)
- **Админка**: `admin.example.com` (замените на ваш домен)

## Шаги развертывания

### 1. Подготовка файлов

1. Скопируйте `.env.example` в `.env` и заполните все переменные:
```bash
cp env.example .env
```

2. В файле `.env` замените домены на ваши:
```env
DOMAIN="ваш-домен.com"
ADMIN_DOMAIN="admin.ваш-домен.com"
```

### 2. Настройка доменов в Timeweb

1. В панели управления Timeweb добавьте ваш основной домен
2. Добавьте поддомен `admin` для админки
3. Настройте DNS записи так, чтобы оба домена указывали на ваш сервер

### 3. Развертывание через Docker Compose

Timeweb поддерживает автоматическое развертывание через Docker Compose:

1. Загрузите все файлы проекта на сервер
2. Убедитесь, что файл `docker-compose.yml` находится в корневой директории
3. Запустите развертывание через панель Timeweb

### 4. Структура сервисов

После развертывания будут запущены следующие сервисы:

- **nginx** (порт 80, 443) - обратный прокси для маршрутизации по доменам
- **store-admin** (внутренний порт 3000) - админская панель
- **store-client** (внутренний порт 3001) - клиентская часть

### 5. Маршрутизация

Nginx автоматически направляет запросы:
- `admin.ваш-домен.com` → store-admin:3000
- `ваш-домен.com` → store-client:3001

### 6. SSL сертификаты (опционально)

Для включения HTTPS:

1. Получите SSL сертификаты (Let's Encrypt или другие)
2. Поместите их в папку `nginx/ssl/`:
   - `cert.pem` - сертификат
   - `key.pem` - приватный ключ
3. Раскомментируйте HTTPS секции в `nginx/nginx.conf`
4. Пересоберите контейнеры

### 7. Мониторинг

Логи nginx доступны в контейнере:
```bash
docker logs store-nginx-1
```

Логи приложений:
```bash
docker logs store-store-admin-1
docker logs store-store-client-1
```

## Переменные окружения

Обязательные переменные для production:

```env
# База данных
DATABASE_URL="postgresql://user:password@host:port/database"

# JWT секрет
JWT_SECRET="your-secure-jwt-secret"

# S3 настройки
S3_URL=https://s3.twcstorage.ru
PICTURES_TRIAL_TEST_BUCKET=your-bucket-name
PICTURES_TRIAL_TEST_BUCKET_S3_ACCESS_KEY=your-access-key
PICTURES_TRIAL_TEST_BUCKET_S3_SECRET_ACCESS_KEY=your-secret-key

# Домены
DOMAIN="ваш-домен.com"
ADMIN_DOMAIN="admin.ваш-домен.com"

# NextAuth
NEXTAUTH_URL="https://admin.ваш-домен.com"
NEXTAUTH_SECRET="your-nextauth-secret"
```

## Обновление

Для обновления приложения:

1. Загрузите новые файлы на сервер
2. Пересоберите контейнеры через панель Timeweb или командой:
```bash
docker-compose up --build -d
```
