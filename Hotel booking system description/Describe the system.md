# Описать систему с помощью UML-диаграмм как: Диаграмма классов, диаграмма последовательностей, ER-диаграмма.

## Основными функциональными требованиями системы являются:

1. Регистрация пользователей и создание профилей: Система позволяет пользователям регистрироваться и создавать свои профили. Владельцы отелей также имеют возможность создать свой профиль, чтобы предоставить информацию о своем отеле.

2. Поиск отелей: Пользователи могут искать отели по странам и городам с помощью удобного интерфейса поиска. Система предоставляет актуальную информацию об отеле, номерах и ценах для бронирования.

3. Бронирование номеров: Пользователи могут бронировать номера с возможностью предоплаты или без нее. Система позволяет выбирать опцию предоплаты или оплаты номера при заселении. Это особенно удобно для гибкого планирования поездок.

4. Роли в системе: В системе предусмотрены две роли - пользователь и админ отеля. Пользователи имеют доступ к функционалу бронирования, а администраторы отелей могут управлять информацией об отеле, номерами и ценами.

5. Оплата комиссии системе: Система предоставляет возможность оплаты комиссии за бронирование. Это позволяет компании получать доход от бронирования и поддерживать работу системы.

6. Получение электронного подтверждения бронирования: После успешного бронирования пользователь получает электронное подтверждение с информацией о бронировании.

7. Интеграция с платежными сервисами: Система интегрируется с различными платежными сервисами, что обеспечивает безопасную и удобную оплату бронирования.

8. Отчетность для администраторов: Система предоставляет администраторам отчетность, которая включает количество забронированных номеров, частоту бронирований и другую полезную информацию. Это позволяет администраторам делать осознанные решения и улучшать управление отелем.

Система для бронирования отелей является онлайн-платформой, которая позволяет пользователям бронировать номера в отелях по всем странам. Ниже приведено описание системы с использованием UML-диаграмм.

## Диаграмма классов:
```python
class User:
    - name: str
    - email: str
    - password: str
    + create_profile()

class HotelOwner:
    - name: str
    - contact_info: str
    - hotel: Hotel
    + create_hotel()

class Hotel:
    - country: str
    - city: str
    - description: str
    - contact_info: str
    + add_room()

class Room:
    - room_type: str
    - description: str
    - price: float

class Booking:
    - user: User
    - hotel: Hotel
    - room: Room
    - check_in_date: date
    - check_out_date: date
    - prepaid: bool
    + confirm_booking()

class Payment:
    - booking: Booking
    + make_payment()

class Confirmation:
    - booking: Booking
    + send_confirmation_email()

class Admin:
    - hotel: Hotel
    + generate_reports()
```

## Диаграмма последовательностей:
```
@startuml
actor User
boundary System
boundary PaymentService
entity User
control System
control PaymentService
participant Hotel
participant Room
participant Reservation
User -> System: Регистрация
User -> System: Поиск отелей
System -> Hotel: Запрос информации
System -> User: Отображение информации
User -> System: Бронирование номера без предоплаты
System -> Hotel: Запрос на бронирование
Hotel -> System: Подтверждение бронирования
System -> Reservation: Создание брони
System -> User: Электронное подтверждение
User -> System: Оплата комиссии
System -> PaymentService: Запрос на платеж
PaymentService -> System: Подтверждение платежа
System -> Reservation: Подтверждение платежа
System -> User: Подтверждение платежа
User -> Hotel: Оплата номера при заселении
Hotel -> System: Подтверждение оплаты
System -> Reservation: Обновление информации о платеже
System -> User: Подтверждение оплаты
@enduml
```
Вкратце:
```
User -> Hotel: create_profile()
HotelOwner -> Hotel: create_hotel()
User -> Hotel: search_hotels()
User -> Hotel: view_hotel_info()
User -> Room: book_room()
Payment -> Booking: make_payment()
Confirmation -> Booking: send_confirmation_email()
Admin -> Booking: generate_reports()
```

Пример:
```
+----------+                +----------+
|  User    |                |  System  |
+----------+                +----------+
    |                            |
    | register()                 |
    |--------------------------->|
    |                            |
    |                            |
    | search_hotels()            |
    |--------------------------->|
    |                            |
    |                            |
    | view_hotel_info()          |
    |--------------------------->|
    |                            |
    |                            |
    | make_reservation()         |
    |--------------------------->|
    |                            |
    |                            |
    | cancel_prepayment()        |
    |--------------------------->|
    |                            |
    | pay_commission()           |
    |--------------------------->|
    |                            |
    |                            |
    | get_confirmation_email()   |
    |--------------------------->|
    |                            |
    |                            |
```

## ER-диаграмма:
```
Сущность User {
  id (PK)
  name
  email
}

Сущность Hotel {
  id (PK)
  name
  country
  city
}

Сущность Room {
  id (PK)
  number
  price
  hotel_id (FK)
}

Сущность Reservation {
  id (PK)
  user_id (FK)
  room_id (FK)
  prepaid
  payment_confirmation
}

User 1--* Reservation
Hotel 1--* Room
Room *--1 Reservation
```

Или такой вид:
```
User (PK: email)
- name
- email
- password

HotelOwner (PK: email)
- name
- email
- contact_info

Hotel (PK: hotel_id, FK: owner_email)
- country
- city
- description
- contact_info

Room (PK: room_id, FK: hotel_id)
- room_type
- description
- price

Booking (PK: booking_id, FK: user_email, FK: hotel_id, FK: room_id)
- check_in_date
- check_out_date
- prepaid

Payment (PK: payment_id, FK: booking_id)
- amount

Confirmation (PK: confirmation_id, FK: booking_id)
- sent_date

Admin (PK: email, FK: hotel_id)

Note: PK - Primary Key, FK - Foreign Key
```

В виде схемы:
```
[User]---(1,1)---(1,*)[Reservation]
                  |
                  |
                  |
            (1,1) [Hotel]---(1,*)[Room]
```

## SQL-запрос для создания таблиц в базе данных:
```SQL
CREATE TABLE User (
  email VARCHAR(255) PRIMARY KEY,
  name VARCHAR(255),
  password VARCHAR(255)
);

CREATE TABLE HotelOwner (
  email VARCHAR(255) PRIMARY KEY,
  name VARCHAR(255),
  contact_info VARCHAR(255)
);

CREATE TABLE Hotel (
  hotel_id INT PRIMARY KEY AUTO_INCREMENT,
  owner_email VARCHAR(255),
  country VARCHAR(255),
  city VARCHAR(255),
  description TEXT,
  contact_info VARCHAR(255),
  FOREIGN KEY (owner_email) REFERENCES HotelOwner(email)
);

CREATE TABLE Room (
  room_id INT PRIMARY KEY AUTO_INCREMENT,
  hotel_id INT,
  room_type VARCHAR(255),
  description TEXT,
  price DECIMAL(10,2),
  FOREIGN KEY (hotel_id) REFERENCES Hotel(hotel_id)
);

CREATE TABLE Booking (
  booking_id INT PRIMARY KEY AUTO_INCREMENT,
  user_email VARCHAR(255),
  hotel_id INT,
  room_id INT,
  check_in_date DATE,
  check_out_date DATE,
  prepaid TINYINT(1),
  FOREIGN KEY (user_email) REFERENCES User(email),
  FOREIGN KEY (hotel_id) REFERENCES Hotel(hotel_id),
  FOREIGN KEY (room_id) REFERENCES Room(room_id)
);

CREATE TABLE Payment (
  payment_id INT PRIMARY KEY AUTO_INCREMENT,
  booking_id INT,
  amount DECIMAL(10,2),
  FOREIGN KEY (booking_id) REFERENCES Booking(booking_id)
);

CREATE TABLE Confirmation (
  confirmation_id INT PRIMARY KEY AUTO_INCREMENT,
  booking_id INT,
  sent_date DATE,
  FOREIGN KEY (booking_id) REFERENCES Booking(booking_id)
);

CREATE TABLE Admin (
  email VARCHAR(255) PRIMARY KEY,
  hotel_id INT,
  FOREIGN KEY (hotel_id) REFERENCES Hotel(hotel_id)
);
```
