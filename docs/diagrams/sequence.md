# Диаграммы последовательностей и активностей

## Диаграмма последовательности 1: Регистрация и вход в систему

**Описание:** Процесс регистрации нового пользователя и входа в систему

```mermaid
sequenceDiagram
    actor Гость as Гость
    participant UI as UI Layer
    participant Auth as AuthController
    participant Service as UserService
    participant DB as Database

    Note over Гость,DB: Основной поток: Вход в систему
    Гость->>UI: Нажимает "Sign In"
    UI->>UI: Показать форму входа
    Гость->>UI: Вводит email и пароль
    UI->>Auth: POST /login
    Auth->>Service: authenticateUser()
    Service->>DB: Проверить учетные данные
    DB-->>Service: Данные пользователя
    Service->>Auth: Успешная аутентификация
    Auth->>UI: Установить сессию, перенаправить
    UI->>Гость: Показать главный экран

    Note over Гость,DB: Альтернативный поток: Регистрация
    Гость->>UI: Нажимает "Sign up here"
    UI->>UI: Показать окно регистрации
    
    Гость->>UI: Вводит данные регистрации
    UI->>Auth: POST /register
    Auth->>Service: registerUser()
    Service->>DB: Проверить уникальность email
    DB-->>Service: Email свободен
    Service->>DB: Сохранить пользователя
    Service->>Auth: Успешная регистрация
    Auth->>UI: Автоматическая аутентификация
    Auth->>UI: Перенаправить на главную
    UI->>Гость: Показать главный экран
```

## Диаграмма последовательности 2: Поиск и просмотр публичных мероприятий

**Описание:** Процесс поиска мероприятий по фильтрам и просмотра деталей мероприятия

```mermaid
sequenceDiagram
    actor Пользователь as Пользователь
    participant UI as UI Layer
    participant EventCtrl as EventController
    participant EventService as EventService
    participant Search as SearchService
    participant DB as Database

    Пользователь->>UI: Выбирает фильтры/время
    Пользователь->>UI: Вводит ключевые слова
    UI->>EventCtrl: GET /events?filters=...
    EventCtrl->>Search: searchEvents()
    Search->>DB: Запрос с фильтрами
    DB-->>Search: Список мероприятий
    Search-->>EventCtrl: Отфильтрованные мероприятия
    EventCtrl-->>UI: Данные для таблицы
    UI->>Пользователь: Показать отфильтрованные мероприятия

    Пользователь->>UI: Нажимает "Event page"
    UI->>EventCtrl: GET /events/{id}
    EventCtrl->>EventService: getEventDetails()
    EventService->>DB: Загрузить мероприятие
    DB-->>EventService: Данные мероприятия
    EventService->>EventService: Загрузить информацию о владельце
    EventService-->>EventCtrl: Полные данные мероприятия
    EventCtrl-->>UI: Данные для страницы мероприятия
    UI->>Пользователь: Показать окно просмотра мероприятия
```

## Диаграмма последовательности 3: Создание мероприятия

**Описание:** Процесс создания нового мероприятия

```mermaid
sequenceDiagram
    actor Пользователь as Пользователь
    participant UI as UI Layer
    participant EventCtrl as EventController
    participant EventService as EventService
    participant DB as Database

    Пользователь->>UI: Нажимает "Add Event"
    UI->>UI: Показать окно создания мероприятия
    
    Пользователь->>UI: Заполняет данные мероприятия
    Пользователь->>UI: Нажимает "Create Event"
    UI->>EventCtrl: POST /events
    EventCtrl->>EventService: createEvent()
    EventService->>DB: Сохранить мероприятие
    DB-->>EventService: ID созданного мероприятия
    EventService-->>EventCtrl: Успешное создание
    EventCtrl-->>UI: Данные созданного мероприятия
    UI->>Пользователь: Показать окно просмотра мероприятия
```
