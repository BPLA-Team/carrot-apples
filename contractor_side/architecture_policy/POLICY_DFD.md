# Итоговая политика архитектуры — DFD

Стрелки показывают разрешённые сообщения или физические разрешения. Идентификаторы F-01…F-10 раскрыты в [спецификации](MESSAGE_CONTRACTS.md). Цвета и принадлежность физических устройств описаны в [таблице доменов](DOMAIN_ASSIGNMENT.md).

```mermaid
stateDiagram-v2
    direction LR
    state "Сервис умного дома" as Service
    state "Локальная панель жильца" as Panel
    state "N · Шлюз сервиса" as N
    state "R · Автоматика" as R
    state "T · Монитор, профиль, статусы" as T
    state "Q · Проверка запросов" as Q
    state "M · Шлюз наблюдения" as M
    state "C · Ограничитель климата" as C
    state "D-02 · Колонка" as K
    state "D-03 · Датчик движения" as D
    state "D-04 · Датчик температуры и влажности" as E
    state "D-05 · Батарея" as B
    state "D-06 · Кондиционер" as A
    state "D-07 · Увлажнитель" as U
    state "D-08 · Камера" as V
    state "D-09 · Пылесос" as W

    Service --> N: F-01 Подписанный профиль
    N --> Service: F-01 Результат
    N --> Q: F-02 Запрос профиля
    Q --> N: F-02 Результат профиля
    K --> Q: F-02 Запрос уборки
    Q --> K: F-02 Статус заявки
    Q --> T: F-03 Проверенный профиль или команда уборки
    T --> Q: F-03 Результат профиля или статус уборки
    E --> T: F-04 Показания
    T --> R: F-04 Показания и активный профиль
    R --> T: F-04 Предложение режима
    T --> C: F-05 Ограниченный во времени запрос режима
    C --> B: F-06 Режим и физический допуск нагрева
    C --> A: F-06 Режим и физический допуск охлаждения
    C --> U: F-06 Режим и физический допуск увлажнения
    T --> W: F-07 Команда уборки
    W --> T: F-07 Отчёт пылесоса
    Panel --> T: F-08 Запрос канонического статуса
    T --> Panel: F-08 Канонический статус
    Panel --> M: F-09 Открытие или закрытие сеанса
    M --> Panel: F-09 Состояние сеанса, видео и отметки
    D --> M: F-10 Событие движения
    M --> V: F-10 Начало или конец выдачи кадров
    V --> M: F-10 Кадры живого видео

    classDef untrusted fill:#ffd6d6,stroke:#a61b1b,color:#2e1010,stroke-width:2px
    classDef trusted fill:#d5f5d9,stroke:#23763a,color:#10381a,stroke-width:2px
    classDef elevating fill:#ffe0b3,stroke:#a54f00,color:#492200,stroke-width:2px
    classDef external fill:#e8eaed,stroke:#59616b,color:#24292f,stroke-width:2px
    class N,R,K,D,E,B,A,U,V,W untrusted
    class T trusted
    class Q,M,C elevating
    class Service,Panel external
```
