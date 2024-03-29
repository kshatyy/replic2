# Домашнее задание к занятию "`Репликация и масштабирование. Часть 2`" - `Шатый Константин`


### Инструкция по выполнению домашнего задания

   1. Сделайте `fork` данного репозитория к себе в Github и переименуйте его по названию или номеру занятия, например, https://github.com/имя-вашего-репозитория/git-hw или  https://github.com/имя-вашего-репозитория/7-1-ansible-hw).
   2. Выполните клонирование данного репозитория к себе на ПК с помощью команды `git clone`.
   3. Выполните домашнее задание и заполните у себя локально этот файл README.md:
      - впишите вверху название занятия и вашу фамилию и имя
      - в каждом задании добавьте решение в требуемом виде (текст/код/скриншоты/ссылка)
      - для корректного добавления скриншотов воспользуйтесь [инструкцией "Как вставить скриншот в шаблон с решением](https://github.com/netology-code/sys-pattern-homework/blob/main/screen-instruction.md)
      - при оформлении используйте возможности языка разметки md (коротко об этом можно посмотреть в [инструкции  по MarkDown](https://github.com/netology-code/sys-pattern-homework/blob/main/md-instruction.md))
   4. После завершения работы над домашним заданием сделайте коммит (`git commit -m "comment"`) и отправьте его на Github (`git push origin`);
   5. Для проверки домашнего задания преподавателем в личном кабинете прикрепите и отправьте ссылку на решение в виде md-файла в вашем Github.
   6. Любые вопросы по выполнению заданий спрашивайте в чате учебной группы и/или в разделе “Вопросы по заданию” в личном кабинете.
   
Желаем успехов в выполнении домашнего задания!
   
### Дополнительные материалы, которые могут быть полезны для выполнения задания

1. [Руководство по оформлению Markdown файлов](https://gist.github.com/Jekins/2bf2d0638163f1294637#Code)

---

### Задание 1

`Активный Master-сервер и Пассивный Репликационный Slave-сервер:`

Высокая Доступность: В случае сбоя на master-сервере, slave-сервер готов к срочному взятию на себя роли мастера, обеспечивая непрерывную работу системы.

Безопасность Данных: Репликация обеспечивает резервное копирование данных, что повышает уровень сохранности информации.

Балансировка Нагрузки: Возможность распределения запросов между master и slave для оптимального использования ресурсов.

`Master-сервер и Несколько Slave-серверов:`


Масштабируемость: Добавление дополнительных slave-серверов позволяет эффективно масштабировать систему для обработки большего количества запросов.

Отказоустойчивость: В случае выхода из строя одного из slave-серверов, другие продолжают работу, минимизируя потери производительности.

Параллелизм: Работа с несколькими slave-серверами позволяет параллельно выполнять операции, ускоряя обработку данных.

`Активный Сервер со Специальным Механизмом Репликации (DRBD):`

Синхронизация в Реальном Времени: DRBD обеспечивает синхронизацию данных в режиме реального времени, что гарантирует последовательность изменений и целостность данных.

Отказоустойчивость: При сбое активного сервера, роль моментально переходит к реплицированной копии, минимизируя простой в работе.

Простота в Управлении: DRBD предоставляет простой и эффективный механизм управления репликацией.

`SAN-кластер:`

Высокая Производительность: SAN-кластеры предоставляют высокоскоростной доступ к общему хранилищу данных, что обеспечивает отличную производительность.

Гибкость и Масштабируемость: Возможность масштабирования системы путем добавления новых узлов кластера.

Централизованное Управление: Управление хранилищем осуществляется централизованно, что облегчает контроль и мониторинг.

### Задание 2

`Шаг 1: Анализ данных и запросов`

Изучение структуры таблиц (пользователи, книги, магазины) и типов запросов, которые часто выполняются.

`Шаг 2: Определение ключевых поля для шаринга`

Выбор ключевых полей, которые будут использоваться для шаринга. Например, можно выбрать поле "user_id" для пользователей, "book_id" для книг, и т.д.

`Шаг 3: Горизонтальный шаринг (по отдельным таблицам)`

Создание отдельных баз данных для каждой таблицы (например, база данных для пользователей, база данных для книг, база данных для магазинов).
Разделение данных на отдельные серверы в зависимости от ключевых полей.

`Шаг 4: Вертикальный шаринг (по колонкам)`

Анализ использования колонок и выделение наиболее часто используемых групп колонок.
Создание отдельных баз данных для каждой группы колонок (например, база данных с базовой информацией о пользователях, база данных с информацией о книгах, и т.д.).

`Шаг 5: Разграничение доступа`

Настройка прав доступа для обеспечения безопасности данных. Разграничение доступа к разным базам данных в зависимости от ролей и функций пользователей.

`Шаг 6: Создание блок-схемы системы`

![Скриншот-1](https://github.com/kshatyy/replic2/blob/main/img/2-1.png)

"App" представляет собой приложение, обращающееся к базе данных.

"Database Sharding Proxy" отвечает за маршрутизацию запросов к соответствующим базам данных.

"Horizontal Sharding" отвечает за разделение данных по ключевым полям между разными серверами.

"Vertical Sharding" представляет отдельные базы данных, разделенные по группам колонок.

`Режимы работы серверов:`

Режим чтения: Серверы могут работать в режиме чтения параллельно, обслуживая запросы на чтение данных.

Режим записи: Записи могут осуществляться на одном из серверов, с последующей синхронизацией изменений между шардами.
