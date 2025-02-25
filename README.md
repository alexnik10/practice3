## Приложения для управления контактами

### Описание проекта  
Приложение разработано для управления контактами, включая хранение информации о телефонах, адресах, email и группах. Пользователи могут создавать, редактировать и удалять контакты, а также организовывать их в группы. Реализованы как REST API, так и UI-интерфейс для работы с данными.  

---

### Запуск проекта с помощью Docker Compose  

Приложение настроено для запуска в контейнерах Docker. В репозитории уже находится файл `docker-compose.yml`, который автоматически разворачивает приложение и базу данных.  

#### Шаги для запуска:  

1. Убедитесь, что у вас установлен **Docker** и **Docker Compose**.  

2. Склонируйте репозиторий:  
   ```bash
   git clone https://github.com/alexnik10/nauJava
   cd nauJava
   ```  

3. Запустите приложение:  
   ```bash
   docker-compose up --build
   ```  
   Этот процесс:  
   - Запустит базу данных PostgreSQL.  
   - Соберёт контейнер приложения.  
   - Запустит приложение, которое будет доступно по адресу: [http://localhost:8080](http://localhost:8080).  

4. Для остановки контейнеров используйте:  
   ```bash
   docker-compose down
   ```  

---

### Основной функционал  
1. **Управление пользователями:**  
   - Регистрация новых пользователей.  
   - Авторизация пользователей для работы с их контактами.  
   
2. **Управление контактами:**  
   - Добавление, редактирование, удаление контактов.  
   - Просмотр списка контактов.  
   - Поиск контактов по группам.  

3. **Управление группами:**  
   - Создание групп и добавление контактов в группы.  
   - Удаление групп.  
   - Получение списка всех групп пользователя.  

4. **Управление дополнительной информацией о контактах:**  
   - Добавление, изменение и удаление телефонов, адресов и email для контактов.  

---

### Технологии и архитектура  

1. **Фреймворк:** Spring Boot  
2. **База данных:** PostgreSQL (через Hibernate и JPA)  
3. **Архитектура:** многослойная (слои: сущности, репозитории, сервисы, контроллеры).  
4. **UI:** Thymeleaf для шаблонов и отображения данных.  
5. **Безопасность:** Spring Security для управления аутентификацией и авторизацией.  
6. **Обработка ошибок:** централизованная обработка через `@ControllerAdvice`.  

---

### Структура приложения  

#### 1. Сущности  

- **User:**  
  Описывает пользователя. Каждый пользователь имеет свои контакты и группы.  

- **Contact:**  
  Представляет контакт с дополнительной информацией (телефоны, адреса, email, группы).  

- **Group:**  
  Группа контактов. Используется для их организации.  

- **Phone, Address, Email:**  
  Дополнительная информация о контакте.  

#### 2. Репозитории  

Используются интерфейсы, наследующие `JpaRepository`, для взаимодействия с базой данных. Например, `ContactRepository` позволяет находить контакты пользователя по его ID или имени.  

#### 3. Сервисы  

Сервисы реализуют бизнес-логику приложения:  
- **`ContactService`**  
  Отвечает за управление контактами пользователей. Этот сервис реализует операции по созданию, редактированию и удалению контактов, а также обеспечивает возможность просмотра списка контактов.  

- **`GroupService`**  
  Управляет группами и их связью с контактами. Например, добавляет контакты в группы, удаляет их из группы и обеспечивает отображение всех контактов, связанных с конкретной группой.  

- **`UserService`**  
  Отвечает за  создание и получение пользователей.  

- **`AddressService`**  
  Реализует управление адресами, включая добавление, обновление и удаление адресов.  

- **`PhoneService`**  
  Управляет телефонными номерами, добавляет или удаляет их.  

- **`EmailService`**  
  Позволяет добавлять, редактировать и удалять электронные адреса.  

#### Проверка целостности данных:  
Для обеспечения безопасности и согласованности данных в каждом сервисе предусмотрены проверки:  
- **Проверка прав доступа**  
  Пользователь может работать только со своей информацией. Например, если пользователь пытается изменить или удалить контакт, который ему не принадлежит, сервис вернёт ошибку, что такого контакта нет.  

- **Связность данных**  
  Например, при удалении контакта удаляются и связанные с ним данные (имейлы, адреса, телефоны).  

#### 4. Контроллеры  

- **API-контроллеры:** REST API для взаимодействия с клиентами.  
- **UI-контроллеры:** для обработки запросов в веб-интерфейсе.  

---
