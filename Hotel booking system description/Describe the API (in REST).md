# Описать API (в REST) получения всех отелей, номеров в отелях, и бронирования. Должно быть включены: примеры запроса/ответа сервера, входные/выходные данные и тд.

## Описание API для получения всех отелей, номеров в отелях и бронирования:

1. Получение всех отелей:
- Метод: GET
- Путь: /hotels
- Входные данные: Отсутствуют
- Выходные данные:
```
[
  {
    "id": 1,
    "name": "Отель 1",
    "country": "Россия",
    "city": "Москва",
    "address": "ул. Примерная, д. 1",
    "phone": "+7-999-123-4567"
  },
  {
    "id": 2,
    "name": "Отель 2",
    "country": "Испания",
    "city": "Мадрид",
    "address": "Calle Ejemplo 2",
    "phone": "+34-999-123-4567"
  },
  ...
]
```

2. Получение всех номеров в определенном отеле:
- Метод: GET
- Путь: /hotels/{hotel_id}/rooms
- Входные данные: Путь содержит идентификатор отеля (hotel_id)
- Выходные данные:
```
[
  {
    "id": 1,
    "hotel_id": 1,
    "number": "101",
    "type": "Стандартный",
    "price": 100,
    "available": true
  },
  {
    "id": 2,
    "hotel_id": 1,
    "number": "201",
    "type": "Люкс",
    "price": 200,
    "available": true
  },
  ...
]
```

3. Бронирование номера в отеле:
- Метод: POST
- Путь: /hotels/{hotel_id}/rooms/{room_id}/booking
- Входные данные:
```
{
  "user_id": 123,
  "payment_method": "prepaid",
  "start_date": "2022-05-01",
  "end_date": "2022-05-05"
}
```
- Выходные данные:
```
{
  "booking_id": 456,
  "hotel_id": 1,
  "room_id": 2,
  "user_id": 123,
  "start_date": "2022-05-01",
  "end_date": "2022-05-05"
}
```

## Python код для создания базы данных и выполнения SQL-запросов:
```python
import sqlite3

# Создание базы данных и таблицы для отелей
conn = sqlite3.connect('hotel_booking.db')
cursor = conn.cursor()

cursor.execute('''
    CREATE TABLE hotels (
        id INTEGER PRIMARY KEY,
        name TEXT,
        country TEXT,
        city TEXT,
        address TEXT,
        phone TEXT
    )
''')

# Таблица для номеров в отелях
cursor.execute('''
    CREATE TABLE rooms (
        id INTEGER PRIMARY KEY,
        hotel_id INTEGER,
        number TEXT,
        type TEXT,
        price REAL,
        available INTEGER,
        FOREIGN KEY (hotel_id) REFERENCES hotels (id)
    )
''')

# Таблица для бронирования номеров
cursor.execute('''
    CREATE TABLE bookings (
        id INTEGER PRIMARY KEY,
        hotel_id INTEGER,
        room_id INTEGER,
        user_id INTEGER,
        start_date TEXT,
        end_date TEXT,
        payment_method TEXT,
        FOREIGN KEY (hotel_id) REFERENCES hotels (id),
        FOREIGN KEY (room_id) REFERENCES rooms (id)
    )
''')

# Пример SQL-запроса на получение всех отелей
cursor.execute('SELECT * FROM hotels')
hotels = cursor.fetchall()
print(hotels)

# Пример SQL-запроса на получение всех номеров в отеле с идентификатором 1
cursor.execute('SELECT * FROM rooms WHERE hotel_id = 1')
rooms = cursor.fetchall()
print(rooms)

# Пример SQL-запроса на бронирование номера
cursor.execute('''
    INSERT INTO bookings (hotel_id, room_id, user_id, start_date, end_date, payment_method)
    VALUES (1, 2, 123, '2022-05-01', '2022-05-05', 'prepaid')
''')
booking_id = cursor.lastrowid
conn.commit()
print(booking_id)

conn.close()
```
