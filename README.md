# 🍕 DeliBot — Telegram WebApp для доставки еды

<div align="center">

![Version](https://img.shields.io/badge/version-1.0.0-blue.svg)
![Python](https://img.shields.io/badge/Python-3.11+-green.svg)
![React](https://img.shields.io/badge/React-18.2-61dafb.svg)
![FastAPI](https://img.shields.io/badge/FastAPI-latest-009688.svg)
![Docker](https://img.shields.io/badge/Docker-Compose-2496ED.svg)
![License](https://img.shields.io/badge/license-MIT-yellow.svg)

**Полнофункциональная платформа для онлайн-заказа и доставки еды через Telegram**

[Демо](https://barakat-cafe.ru) • [Установка](#-установка) • [Документация](docs/README.md) • [API](#-api-документация)

</div>

---

## 📋 Содержание

- [О проекте](#-о-проекте)
- [Возможности](#-возможности)
- [Технологический стек](#-технологический-стек)
- [Архитектура](#-архитектура)
- [Структура проекта](#-структура-проекта)
- [Установка](#-установка)
- [Конфигурация](#-конфигурация)
- [Запуск](#-запуск)
- [API документация](#-api-документация)
- [База данных](#-база-данных)
- [Разработка](#-разработка)
- [Документация](#-документация)

---

## 🎯 О проекте

**DeliBot** — это современное решение для автоматизации заказов и доставки еды, интегрированное с Telegram. Проект включает:

- 📱 **Клиентское WebApp** — интерфейс для покупателей прямо в Telegram
- 🖥️ **Админ-панель** — управление заказами, меню, статистикой
- 🤖 **Клиентский бот** — взаимодействие с пользователями
- 👨‍💼 **Админ-бот** — уведомления и управление заказами для персонала
- ⚙️ **Backend API** — REST API на FastAPI
- 🗄️ **PostgreSQL + PostGIS** — хранение данных с поддержкой геолокации

---

## ✨ Возможности

### Для клиентов

- 🛒 Просмотр каталога с категориями и поиском
- 🍔 Детальные карточки товаров с ингредиентами
- ➕ Выбор опциональных ингредиентов (компоненты)
- 🛍️ Корзина с изменением количества
- 📍 Управление адресами доставки с картой
- 🚗 Доставка или самовывоз
- 💳 Онлайн-оплата через YooKassa
- 📊 История заказов и отслеживание статуса
- 💰 Программа лояльности (10% кэшбэк баллами)
- 🎟️ Промокоды
- 📰 Лента новостей

### Для администраторов

- 📈 Дашборд со статистикой и графиками
- 📦 Управление заказами в реальном времени
- 🍕 CRUD операции для меню (товары, категории, ингредиенты)
- 👥 Управление пользователями
- 📍 Настройка зон доставки
- 💬 Настройка сообщений бота
- ⚙️ Системные настройки
- 📊 Аналитика продаж

### Технические

- 🔐 JWT аутентификация
- 🗺️ Геолокация с PostGIS
- 📨 Telegram уведомления
- 🔄 Webhook интеграция
- 📱 Адаптивный дизайн
- 🌐 Мультиязычность (подготовлено)

---

## 🛠 Технологический стек

### Frontend

| Технология    | Версия | Назначение                      |
| ------------- | ------ | ------------------------------- |
| React         | 18.2   | UI библиотека                   |
| TypeScript    | 5.3    | Типизация                       |
| Vite          | 5.4    | Сборщик                         |
| TailwindCSS   | 3.4    | Стилизация                      |
| SCSS Modules  | —      | Компонентные стили              |
| React Query   | 5.x    | Управление серверным состоянием |
| Zustand       | 5.x    | Глобальное состояние            |
| React Router  | 6.x    | Маршрутизация                   |
| Framer Motion | 12.x   | Анимации                        |
| Recharts      | 3.x    | Графики (админка)               |
| Zod           | 4.x    | Валидация                       |

### Backend

| Технология   | Версия | Назначение         |
| ------------ | ------ | ------------------ |
| Python       | 3.11+  | Язык               |
| FastAPI      | latest | REST API фреймворк |
| SQLAlchemy   | 2.x    | ORM                |
| Alembic      | latest | Миграции БД        |
| aiogram      | 3.x    | Telegram Bot API   |
| YooKassa SDK | latest | Платежи            |
| Redis        | 7.x    | Кеширование, FSM   |
| APScheduler  | —      | Фоновые задачи     |

### Инфраструктура

| Технология              | Назначение                |
| ----------------------- | ------------------------- |
| PostgreSQL 15 + PostGIS | База данных с геолокацией |
| Redis 7                 | Кеш и хранилище сессий    |
| Docker Compose          | Оркестрация контейнеров   |
| Nginx                   | Reverse proxy, статика    |
| Yandex Object Storage   | Хранение изображений      |

---

## 🏗 Архитектура

```
┌─────────────────────────────────────────────────────────────────┐
│                         TELEGRAM                                 │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────────────┐  │
│  │ Client Bot   │  │  Admin Bot   │  │   WebApp (iframe)    │  │
│  │   (aiogram)  │  │   (aiogram)  │  │   React + TS         │  │
│  └──────┬───────┘  └──────┬───────┘  └──────────┬───────────┘  │
└─────────┼─────────────────┼─────────────────────┼───────────────┘
          │                 │                     │
          ▼                 ▼                     ▼
┌─────────────────────────────────────────────────────────────────┐
│                      BACKEND LAYER                               │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │                    FastAPI Backend                        │   │
│  │  ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────────────┐ │   │
│  │  │ Routers │ │Services │ │Telegram │ │   Scheduler     │ │   │
│  │  │  (REST) │ │ (Logic) │ │Webhooks │ │  (Background)   │ │   │
│  │  └─────────┘ └─────────┘ └─────────┘ └─────────────────┘ │   │
│  └──────────────────────────────────────────────────────────┘   │
│                              │                                   │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │                     CORE Layer                            │   │
│  │  ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────────────┐ │   │
│  │  │ Models  │ │Requests │ │Services │ │   External      │ │   │
│  │  │  (ORM)  │ │ (CRUD)  │ │(Business│ │  (YooKassa,     │ │   │
│  │  │         │ │         │ │  Logic) │ │   Geocoder)     │ │   │
│  │  └─────────┘ └─────────┘ └─────────┘ └─────────────────┘ │   │
│  └──────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────────┐
│                      DATA LAYER                                  │
│  ┌────────────────────┐        ┌────────────────────┐          │
│  │  PostgreSQL 15     │        │      Redis 7       │          │
│  │  + PostGIS         │        │  (Cache, FSM)      │          │
│  │  (Main Database)   │        │                    │          │
│  └────────────────────┘        └────────────────────┘          │
└─────────────────────────────────────────────────────────────────┘
```

---

## 📁 Структура проекта

```
Telegram_Bot_3/
├── 📂 admin/                    # Telegram Admin Bot
│   ├── app/
│   │   ├── handlers/           # Обработчики команд
│   │   ├── keyboards/          # Клавиатуры
│   │   ├── middlewares/        # Middleware
│   │   ├── states/             # FSM состояния
│   │   └── main.py             # Точка входа
│   ├── Dockerfile
│   └── requirements.txt
│
├── 📂 admin-frontend/           # React Admin Panel
│   ├── src/
│   │   ├── app/                # Providers, routing
│   │   ├── pages/              # Страницы
│   │   │   ├── DashboardPage/  # Главная со статистикой
│   │   │   ├── MenuPage/       # Управление меню
│   │   │   ├── UsersPage/      # Пользователи
│   │   │   ├── SettingsPage/   # Настройки
│   │   │   └── ...
│   │   ├── features/           # Фичи (таблицы, формы)
│   │   ├── shared/             # Общие компоненты
│   │   └── widgets/            # Виджеты
│   ├── Dockerfile
│   └── package.json
│
├── 📂 backend/                  # FastAPI Backend
│   ├── app/
│   │   ├── routers/            # API endpoints
│   │   │   ├── orders/         # Заказы
│   │   │   ├── products/       # Товары
│   │   │   ├── users/          # Пользователи
│   │   │   ├── payments/       # Платежи
│   │   │   ├── statistics/     # Статистика
│   │   │   └── ...
│   │   ├── dependencies/       # DI зависимости
│   │   ├── middlewares/        # Middleware
│   │   ├── scheduler/          # Фоновые задачи
│   │   ├── telegram/           # Telegram интеграция
│   │   └── main.py             # Точка входа
│   ├── Dockerfile
│   └── requirements.txt
│
├── 📂 core/                     # Shared Business Logic
│   ├── config/                 # Конфигурации
│   ├── db/                     # Database connection
│   ├── models/                 # SQLAlchemy Models
│   │   ├── user/               # Пользователи, админы
│   │   ├── product/            # Товары, категории
│   │   ├── order/              # Заказы
│   │   ├── payment/            # Платежи
│   │   ├── address/            # Адреса
│   │   ├── cart/               # Корзина
│   │   ├── loyalty/            # Лояльность
│   │   └── ...
│   ├── request/                # CRUD операции
│   ├── services/               # Бизнес-сервисы
│   │   ├── yookassa_service.py # Платежи
│   │   ├── telegram_service.py # Уведомления
│   │   └── ...
│   └── external/               # Внешние API
│
├── 📂 frontend/                 # React Client WebApp
│   ├── src/
│   │   ├── app/                # Providers, routing
│   │   ├── pages/              # Страницы
│   │   │   ├── home/           # Главная (каталог)
│   │   │   ├── cart/           # Корзина
│   │   │   ├── order/          # Оформление заказа
│   │   │   ├── product-details/# Детали товара
│   │   │   ├── ProfilePage/    # Профиль
│   │   │   ├── settings/       # Настройки
│   │   │   └── ...
│   │   ├── features/           # Фичи
│   │   ├── shared/             # Общие компоненты
│   │   └── widgets/            # Виджеты
│   ├── Dockerfile
│   └── package.json
│
├── 📂 main/                     # Telegram Client Bot
│   ├── app/
│   │   ├── handlers/           # Обработчики
│   │   ├── keyboards/          # Клавиатуры
│   │   └── main.py
│   ├── Dockerfile
│   └── requirements.txt
│
├── 📂 database/                 # Database Init
│   └── init.sql                # Схема БД
│
├── 📂 alembic/                  # Migrations
│   ├── versions/               # Файлы миграций
│   └── env.py
│
├── 📂 scripts/                  # Утилиты
│   ├── commit.bat              # Git commit helper
│   └── count_lines.py          # Подсчёт строк кода
│
├── 📄 docker-compose.yml        # Оркестрация
├── 📄 alembic.ini               # Конфиг миграций
├── 📄 .env                      # Переменные окружения
└── 📄 README.md                 # Этот файл
```

---

## 🚀 Установка

### Требования

- Docker & Docker Compose v2+
- Git
- (Опционально) Node.js 18+ для локальной разработки
- (Опционально) Python 3.11+ для локальной разработки

### Клонирование репозитория

```bash
git clone https://github.com/your-username/telegram-bot-delivery.git
cd telegram-bot-delivery
```

### Настройка переменных окружения

```bash
# Скопируйте пример конфигурации
cp .env.example .env

# Отредактируйте .env файл
nano .env  # или используйте любой редактор
```

---

## ⚙️ Конфигурация






## ▶️ Запуск

### Production (Docker Compose)

```bash
# Сборка и запуск всех сервисов
docker compose up --build -d

# Просмотр логов
docker compose logs -f

# Просмотр логов конкретного сервиса
docker compose logs -f backend

# Остановка
docker compose down
```

### Сервисы и порты

| Сервис            | Порт | URL                   |
| ----------------- | ---- | --------------------- |
| Frontend (клиент) | 8080 | http://localhost:8080 |
| Admin Frontend    | 5181 | http://localhost:5181 |
| Backend API       | 8000 | http://localhost:8000 |
| PostgreSQL        | 5432 | (внутренний)          |
| Redis             | 6379 | (внутренний)          |

### Локальная разработка

#### Frontend

```bash
cd frontend
npm install
npm run dev  # http://localhost:5173
```

#### Admin Frontend

```bash
cd admin-frontend
npm install
npm run dev  # http://localhost:5174
```

#### Backend

```bash
cd backend
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate
pip install -r requirements.txt
uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
```

---

## 📚 API документация

После запуска backend доступна автоматическая документация:

- **Swagger UI**: http://localhost:8000/docs
- **ReDoc**: http://localhost:8000/redoc
- **OpenAPI JSON**: http://localhost:8000/openapi.json

### Основные endpoints

```
# Пользователи
GET    /users/                    # Список пользователей
GET    /users/{telegram_id}       # Получить пользователя
POST   /users/                    # Создать пользователя

# Продукты
GET    /products/                 # Каталог товаров
GET    /products/{id}             # Детали товара
POST   /products/                 # Создать товар (admin)
PUT    /products/{id}             # Обновить товар (admin)

# Категории
GET    /categories/               # Список категорий
POST   /categories/               # Создать категорию (admin)

# Заказы
GET    /orders/                   # Список заказов
GET    /orders/{id}               # Детали заказа
POST   /orders/                   # Создать заказ
PATCH  /orders/{id}/status        # Обновить статус

# Корзина
GET    /cart/{user_id}            # Получить корзину
POST   /cart/add                  # Добавить товар
DELETE /cart/remove               # Удалить товар

# Платежи
POST   /payments/create           # Создать платёж
POST   /payments/webhook          # YooKassa webhook

# Статистика (admin)
GET    /statistics/dashboard      # Данные дашборда
GET    /statistics/sales          # Продажи
```

---

## 🗄 База данных

### Основные таблицы

```sql
-- Пользователи
users (id, telegram_id, username, phone, loyalty_points, ...)

-- Товары и категории
categories (id, name, description, order_num, ...)
products (id, name, description, price, category_id, image_url, ...)
ingredients (id, name, type, is_selectable, price, ...)
product_ingredients (product_id, ingredient_id, amount, ...)

-- Заказы
orders (id, user_id, status, total, delivery_method, ...)
order_items (id, order_id, product_id, quantity, price, ...)
payments (id, order_id, payment_id, status, amount, ...)

-- Адреса
addresses (id, user_id, label, city, street, latitude, longitude, ...)
delivery_zones (id, name, area, delivery_cost, min_order_amount, ...)

-- Прочее
carts, cart_items, news, promo_codes, product_ratings, ...
```

### Миграции

```bash
# Создать новую миграцию
alembic revision --autogenerate -m "description"

# Применить миграции
alembic upgrade head

# Откатить последнюю миграцию
alembic downgrade -1

# Посмотреть историю
alembic history
```

---

## 👨‍💻 Разработка

### Структура коммитов

```bash
# Используйте скрипт для коммитов
./scripts/commit.bat "feat: добавлена новая функция"
```

### Подсчёт строк кода

```bash
python scripts/count_lines.py
```

### Полезные команды

```bash
# Пересборка конкретного сервиса
docker compose up --build frontend

# Вход в контейнер
docker compose exec backend bash

# Просмотр состояния контейнеров
docker compose ps

# Очистка Docker
docker system prune -a --volumes
```

---

## 📊 Статистика проекта

| Метрика          | Значение      |
| ---------------- | ------------- |
| Всего строк кода | ~79,000+      |
| Файлов           | 904           |
| TypeScript       | ~42,700 строк |
| Python           | ~14,000 строк |
| CSS/SCSS         | ~22,000 строк |

---

## 🔒 Безопасность

- ✅ JWT токены для аутентификации
- ✅ SQL Injection защита
- ✅ CORS настройка
- ✅ Хеширование паролей (bcrypt)
---

