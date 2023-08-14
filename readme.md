# Тестовое задание в компанию MEDODS

## Исходная информация по заданию:

### Используемые технологии
- GO
- JWT
- MongoDB

### Задание
Написать часть сервиса аутентификации.
Два REST маршрута:
- Первый маршрут выдает пару Access, Refresh токенов для пользователя сидентификатором (GUID) указанным в параметре запроса
- Второй маршрут выполняет Refresh операцию на пару Access, Refreshтокенов

### Требования
Access токен тип JWT, алгоритм SHA512, хранить в базе строго запрещено.

Refresh токен тип произвольный, формат передачи base64, хранится в базеисключительно в виде bcrypt хеша, должен быть защищен от изменения настороне клиента и попыток повторного использования.

Access, Refresh токены обоюдно связаны, Refresh операцию для Access токена можно выполнить только тем Refresh токеном который был выдан вместе с ним.

### Результат
Результат выполнения задания нужно предоставить в виде исходного кода на Github.

---

## Комментарии к исполнению и рассуждения:
- Для роутинга буду использовать [Echo](https://echo.labstack.com/) роутер
- Для конфигов буду использовать [Viper](https://github.com/ory/viper)
- Для работа с JWT использую [dgrijalva/jwt-go](https://github.com/dgrijalva/jwt-go)

- Перепрочитал задание, понял что сделал не так, пишу как понял.
Я делаю сервис аутентификации, в базе я буду хранить id юзера с хэш его refresh токена, чтобы подтверждать - принадлежит ли этот токен пользователю.
- Как я понял, refresh токен достаточно сделать просто рандомно-сгенерированной строкой, хеш которой будет в базе данных, и операция refresh будет их пересоздавать чтобы оставались связанными, т.е. нельзя будет использовать старый refresh потому что он замениться новым refresh. Надеюсь правильно понял задание.

- Теперь не знаю откуда получать id пользователя, видимо refresh операцию могу делать только авторизированные пользователи и мне нужно прикерпить еще и считывать access токен, теперь их связь точно прочна, значит следить за тем чтобы acces токен не истек будет клиент. Продлить access токен не изменив его невозможно, без хранения expires например в базе, тогда лучше выдавать на такую операцию новую пару access и refresh токенов. Так и поступлю.

- Описать краткую струткуру по приложению:
    - cmd -> main.go
    - config -> config
    - logger -> общий интерфейс логгера, я логи понаставлял и где то не убрал не бейте плиз
    - model -> общие ошибки и структура сессии
    - repository -> манагер репозитория и слой работы с монго
    - service -> ***генератор JWT***, логика
    - transport -> сервер и 2 хендлера

- Присутствуют так же 2 sh файла, попытался сделать сценарии, доработаю пока
- В makefile единственная команда для создания моков
- В app.env лежит ***MONGO_URI***, подключаеться к моему кластеру на атласе, секреты конечно хранить так не гоже, но проект то тестовый

- Надеюсь на обратную связь по заданию, елси вы дадите мне вектор развития или какие книжки почитать, у меня на [books](https://github.com/ynuraddi/prog-books), есть список, можете закидывать через issue
- Есть моменты с которыми не разобрался, но сейчас у меня не хватает времени, жестко учу алгоритмы, мой прогресс вы так можете посомтреть [здесь](https://github.com/ynuraddi/data-strutures). Пока маловато, но все будет.

- Удачи вам и мне)!