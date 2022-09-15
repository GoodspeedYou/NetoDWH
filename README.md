# NetoDWH
Final neto dwh 


Объединяющий трансформации Job - dwh_final.kjb

Справочник dim_aircrafts создается в трансформации dwh_aircrafts.ktr
Описание алгоритма ETL:
- Загружаются данные из таблицы базы bookings.aircrafts_data
- Разнос json полей на отдельные колонки
- Выбор полей для справочника (исключаем поле с json)
- Подсчет длинны кода самолета для проверки корректности формата (представляем что должен состоять из 3 символов)
- Проверка строк на заполненность
- Проверка заполненности модели хотябы на одном языке
- Проверка корректной дальности перелета (>100)
- Проверка формата кода самолета
- Запись строк проваливших проверки в rejected-csv файл 
- Запись валидных строк в таблицу базы zen.dim_aircrafts и csv файл


Справочник dim_airports создается в трансформации dwh_airports.ktr
Описание алгоритма ETL:
- Загружаются данные из таблицы базы bookings.airports_data
- Разнос json полей на отдельные колонки (в 2 шага тк 2 json поля)
- Подсчет длинны кода аэропорта для проверки корректности формата (представляем что должен состоять из 3 символов)
- Проверка строк на заполненность
- Проверка формата кода аэропорта
- Запись строк проваливших проверки в rejected-csv файл 
- Запись валидных строк в таблицу базы zen.dim_airports и csv файл


Справочник dim_passangers создается в трансформации dwh_passengers.ktr
Описание алгоритма ETL:
- Загружаются данные из таблицы базы bookings.tickets
- Выбор полей для справочника (исключаем поля связные с билетами)
- Разнос json полей на отдельные колонки
- Выбор полей для справочника (исключаем поле с json)
- Проверка строк на заполненность (поле email считаем не обязательным)
- Запись строк проваливших проверки в rejected-csv файл 
- Запись валидных строк в таблицу базы zen.dim_airports и csv файл
- Запись строк проваливших проверки в rejected-csv файл 
- Запись валидных строк в таблицу базы zen.dim_passengers и csv файл


Справочник фактов fact_flights создается в трансформации dwh_flights.ktr
Описание алгоритма ETL:
- Загружаются данные из таблицы базы bookings.flights
- Подсчитываются поля переносов вылета и прилета delay_departure и delay_arrival
- Присоединяются данные о самолете из таблицы bookings.aircrafts_data с предварительным разносом json поля
- Присоединяются данные о аэропорте вылета/прилета из таблицы bookings.airports_data с предварительным разносом json поля
- К данным о билетах таблицы bookings.ticket_fligts присоединяются полученные поля. Получаем факты перелета для каждого билета с данными стоимости и классом
- Присоединяются данные о пассажире (имя) из таблицы bookings.tickets
- Проверка завершенности вылета
- Выбор полей для справочника
- Присоединяем текущую дату для дальнейшей проверки корректности
- Запись строк проваливших проверки в rejected-csv файл с предварительным выбором полей
- Запись валидных строк в таблицу базы zen.fact_flights и csv файл


SQL по созданию справочника хранятся в файле Script.sql
В нем:
- Создается схема zen
- Создаются справочники для наших трансформация fact_flights, dim_passangers, dim_airports, dim_aircrafts 
- Генерируется календарь dim_calendar

В jpeg файлах ER_shema_bookings и ER_shema_zen представлены скриншоты ER-диаграмм схем базы и справочников
В jpeg файлах dwh_final_job, dwh_flights, dwh_airports, dwh_aircrafts, dwh_passengers представленны скриншоты ETL-процессов

В соответствующих названиям справочников csv файлах хранятся валидные данные. В csv файлах с меткой rejected данные не прошедшие проверки
