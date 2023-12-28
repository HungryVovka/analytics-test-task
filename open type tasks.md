# Система бронирования отеля:
```
Кейс:
Крупная компания планирует создать Систему для бронирования отелей. Система позволяет бронировать
номера в отелях по всем странам. В системе можно бронировать номера в отелях,
с возможностью предоплаты и без.

Список функциональных требований:
1. Регистрация пользователей и создание профилей как для пользователей, так и для владельцев отелей;
2. Поиск отелей по странам и городам;
3. Просмотр информации об отеле, номерах и ценах для бронирования;
4. Бронирование номеров с возможностью предоплаты или без;
5. Роли в системе: пользователь, админ отеля;
6. Возможность отказаться от предоплаты и оплатить номер при заселении;
7. Оплата комиссии системе за бронирование;
8. Получение электронного подтверждения бронирования;
9. Интеграция с системами платежных сервисов;
10. Отчетность для администраторов, описывающая количество забронированных номеров, частоту бронирований и т.д.

Задание:
    1. Описать систему с помощью UML-диаграмм как: Диаграмма классов, диаграмма последовательностей, ER-диаграмма.
    2. Описать API (в REST) получения всех отелей, номеров в отелях, и бронирования.
    Должно быть включены: примеры запроса/ответа сервера, входные/выходные данные и тд.
    3. Описать словами, как бы вы построили работу команды с точки зрения подхода к разработке.
    Вопрос не обязателен, но желателен.
```

### Для создания системы бронирования отелей, удовлетворяющей перечисленным функциональным требованиям, можно использовать следующие технологии и языки программирования:

1. Бэкенд-разработка:
   - Язык программирования: Python, Java, C# или Node.js для разработки серверной части.
   - Фреймворки: Django (Python), Spring (Java), ASP.NET (C#), Express.js (Node.js) для упрощения разработки бэкенда.
   - База данных: PostgreSQL, MySQL или MongoDB для хранения информации об отелях, пользователях, бронированиях и т.д.

2. Фронтенд-разработка:
   - Языки программирования: HTML, CSS, JavaScript (или TypeScript) для создания интерфейса пользователя.
   - Фреймворки: React, Angular, Vue.js для разработки динамического веб-интерфейса.
   - Инструменты: Webpack, Babel для сборки и транспиляции кода.

3. Интеграция с платежными сервисами:
   - Использование API платежных сервисов (например, Stripe, PayPal, Braintree) для обработки платежей.

4. Отчетность:
   - Использование фреймворков для формирования отчетов, таких как JasperReports, Crystal Reports или разработка собственного функционала.
  
5. Регистрация и аутентификация пользователей:
   - Использование библиотек для аутентификации и авторизации, таких как OAuth, JWT (JSON Web Tokens) или библиотеки соответствующих фреймворков.

Для управления проектом и его компонентами можно использовать системы контроля версий, такие как Git, и платформы для управления проектами, например, Jira или Trello

### Описать систему с помощью UML-диаграмм как: Диаграмма классов, диаграмма последовательностей, ER-диаграмма.

Основными функциональными требованиями системы являются:

1. Регистрация пользователей и создание профилей: Система позволяет пользователям регистрироваться и создавать свои профили. Владельцы отелей также имеют возможность создать свой профиль, чтобы предоставить информацию о своем отеле.

2. Поиск отелей: Пользователи могут искать отели по странам и городам с помощью удобного интерфейса поиска. Система предоставляет актуальную информацию об отеле, номерах и ценах для бронирования.

3. Бронирование номеров: Пользователи могут бронировать номера с возможностью предоплаты или без нее. Система позволяет выбирать опцию предоплаты или оплаты номера при заселении. Это особенно удобно для гибкого планирования поездок.

4. Роли в системе: В системе предусмотрены две роли - пользователь и админ отеля. Пользователи имеют доступ к функционалу бронирования, а администраторы отелей могут управлять информацией об отеле, номерами и ценами.

5. Оплата комиссии системе: Система предоставляет возможность оплаты комиссии за бронирование. Это позволяет компании получать доход от бронирования и поддерживать работу системы.

6. Получение электронного подтверждения бронирования: После успешного бронирования пользователь получает электронное подтверждение с информацией о бронировании.

7. Интеграция с платежными сервисами: Система интегрируется с различными платежными сервисами, что обеспечивает безопасную и удобную оплату бронирования.

8. Отчетность для администраторов: Система предоставляет администраторам отчетность, которая включает количество забронированных номеров, частоту бронирований и другую полезную информацию. Это позволяет администраторам делать осознанные решения и улучшать управление отелем.

Система для бронирования отелей является онлайн-платформой, которая позволяет пользователям бронировать номера в отелях по всем странам. Ниже приведено описание системы с использованием UML-диаграмм.

Диаграмма классов:
```
@startuml
class User {
  -id: int
  -name: str
  -email: str
}

class Hotel {
  -id: int
  -name: str
  -country: str
  -city: str
  -rooms: List[Room]
}

class Room {
  -id: int
  -number: int
  -price: float
}

class Reservation {
  -id: int
  -user: User
  -hotel: Hotel
  -room: Room
  -prepaid: bool
  -payment_confirmation: bool
}

class System {
  -users: List[User]
  -hotels: List[Hotel]
  +search_hotels(country: str, city: str) : List[Hotel]
  +get_hotel_info(hotel_id: int) : Hotel
  +get_room_info(room_id: int) : Room
  +reserve_room(user_id: int, room_id: int, prepaid: bool) : Reservation
  +cancel_reservation(reservation_id: int) : bool
  +pay_commission(reservation_id: int) : bool
  +get_reservation_confirmation(reservation_id: int) : bool
  +generate_report() : Report
}

class HotelAdmin {
  -hotel: Hotel
  +add_room(number: int, price: float) : Room
}

class PaymentService {
  +process_payment(amount: float) : bool
}

class Report {
  -reserved_rooms: int
  -booking_frequency: float
}
User --> Reservation
User o-- HotelAdmin
Hotel --> Room
Reservation --> Room
Reservation --> User
Reservation --> Hotel
HotelAdmin --> Hotel
System --> User
System --> Hotel
System --> Reservation
System --> Report
PaymentService --> Reservation
@enduml
```

Диаграмма последовательностей:
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

ER-диаграмма:
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