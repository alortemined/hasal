# hasal
HI SEMEN HASAL
WAKE UP

CREATE TABLE users (
    users_id INT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    username VARCHAR(255) UNIQUE NOT NULL,
    password_hash TEXT NOT NULL,
    role VARCHAR(50) NOT NULL
);

CREATE TABLE clients (
    clients_id SERIAL PRIMARY KEY,
    surname VARCHAR(255) NOT NULL,
    name VARCHAR(255) NOT NULL,
    patronymic VARCHAR(255),
    contact_phone VARCHAR(20) NOT NULL UNIQUE,
    email VARCHAR(255) UNIQUE NOT NULL UNIQUE,
    contract_number VARCHAR(50) UNIQUE NOT NULL,
    contract_date DATE NOT NULL,
    users_id INT REFERENCES users(users_id),
    adress TEXT
);

CREATE TABLE address_clients (
    address_clients_id SERIAL PRIMARY KEY,
    clients_id INT REFERENCES clients(clients_id) ON DELETE CASCADE,
    city VARCHAR(100) NOT NULL,
    street VARCHAR(255) NOT NULL,
    house VARCHAR(20) NOT NULL,
    flat VARCHAR(20),
    floor VARCHAR(20)
);

CREATE TABLE services (
    services_id SERIAL PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    description TEXT,
    duration INTERVAL NOT NULL,
    price DECIMAL(10, 2) NOT NULL,
    status VARCHAR(50) NOT NULL
);

CREATE TABLE orders (
    orders_id SERIAL PRIMARY KEY,
    clients_id INT REFERENCES clients(clients_id) ON DELETE CASCADE,
    services_id INT REFERENCES services(services_id) ON DELETE SET NULL,
    orders_date DATE NOT NULL,
    status VARCHAR(50) NOT NULL,
    total_price DECIMAL(10, 2) NOT NULL
);

ALTER TABLE orders
ADD COLUMN employees_id INT REFERENCES employees(employees_id) ON DELETE SET NULL;

CREATE TABLE invoices (
    invoices_id SERIAL PRIMARY KEY,
    orders_id INT REFERENCES orders (orders_id) ON DELETE CASCADE,
    issue_date DATE NOT NULL,
    due_date DATE NOT NULL,
    total_amount DECIMAL(10, 2) NOT NULL,
    status VARCHAR(50) NOT NULL
);

CREATE TABLE security_posts (
    security_posts_id SERIAL PRIMARY KEY,
    adress TEXT NOT NULL,
    type VARCHAR(50) NOT NULL,
    security_level VARCHAR(50) NOT NULL,
    status VARCHAR(50) NOT NULL
);

CREATE TABLE security_posts_and_orders (
    security_posts_and_orders_id SERIAL PRIMARY KEY,
    orders_id INT REFERENCES orders (orders_id) ON DELETE CASCADE,
    security_posts_id INT REFERENCES security_posts(security_posts_id) ON DELETE CASCADE
);

CREATE TABLE equipments (
    equipments_id SERIAL PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    type VARCHAR(50) NOT NULL,
    status VARCHAR(50) NOT NULL,
    assigned_to TEXT
);

CREATE TABLE security_posts_and_equipments (
    security_posts_id INT REFERENCES security_posts(security_posts_id) ON DELETE CASCADE,
    equipments_id INT REFERENCES equipments(equipments_id) ON DELETE CASCADE,
    PRIMARY KEY (security_posts_id, equipments_id)
);

CREATE TABLE employees (
    employees_id SERIAL PRIMARY KEY,
    surname VARCHAR(255) NOT NULL,
    name VARCHAR(255) NOT NULL,
    patronymic VARCHAR(255),
    phone_number VARCHAR(20) NOT NULL UNIQUE,
    email VARCHAR(255) UNIQUE NOT NULL UNIQUE,
    hire_date DATE NOT NULL,
    status VARCHAR(50) NOT NULL,
    users_id INT REFERENCES users(users_id)
);

CREATE TABLE employees_assignments (
    employees_assignments_id SERIAL PRIMARY KEY,
    orders_id INT REFERENCES orders (orders_id) ON DELETE CASCADE,
    employees_id INT REFERENCES employees(employees_id) ON DELETE CASCADE,
    assigned_date DATE NOT NULL,
    shift_start TIME NOT NULL,
    shift_end TIME NOT NULL
);

CREATE TABLE incidents_reports (
    incidents_reports_id SERIAL PRIMARY KEY,
    security_posts_and_orders_id INT REFERENCES security_posts_and_orders(security_posts_and_orders_id) ON DELETE CASCADE,
    employees_id INT REFERENCES employees(employees_id) ON DELETE SET NULL,
    report_date DATE NOT NULL,
    description TEXT NOT NULL,
    status VARCHAR(50) NOT NULL
);

CREATE TABLE security_posts_and_services (
    services_id INT REFERENCES services(services_id) ON DELETE CASCADE,
    employees_id INT REFERENCES employees(employees_id) ON DELETE CASCADE,
    PRIMARY KEY (services_id, employees_id)
);

INSERT INTO users (username, password_hash, role) VALUES
('ketsyu', 'dcvfbghnmj', 'Админ'),
('hasal', 'wertgyhjukmnjhb', 'Админ'),
('padpa', 'poiugnmbv', 'Менеджер'),
('frdvl', 'olkndrtghn', 'Менеджер'),
('ersvl', '456349jkfyh', 'Клиент'),
('kudal', '0987bhbncd', 'Клиент'),
('gazru', 'edcvbhyuj', 'Клиент'),
('fease', 'lkjhdrtg', 'Клиент'),
('addan', 'qsdfbnmo', 'Клиент'),
('praev', 'qweasd123', 'Клиент');

INSERT INTO clients (surname, name, patronymic, contact_phone, email, contract_number, contract_date, users_id, adress) VALUES
('Иванов', 'Иван', 'Николаевич', '891242134', 'ivanov1@mail.com', 'ДОГ001', '2024-01-01', 11, 'Улица Суворова, Город Томск'),
('Петров', 'Петр', 'Иванович', '893246234', 'petrov1@mail.com', 'ДОГ002', '2024-01-02', 12, 'Улица Клименто, Город Космический'),
('Сидоров', 'Сидор', 'Петрович', '7891412421', 'sidorov1@mail.com', 'ДОГ003', '2024-01-03', 13, 'Улица Великого, Город Великий'),
('Козлов', 'Алексей', 'Сидорович', '891367812', 'kozlov1@mail.com', 'ДОГ004', '2024-01-04', 14, 'Улица Прекрасного, Город Красоты'),
('Смирнов', 'Дмитрий', 'Алексеевич', '8913000000', 'smirnov1@mail.com', 'ДОГ005', '2024-01-05', 15, 'Улица Чудесная, Город Чудес'),
('Волков', 'Сергей', 'Дмитриевич', '8956234234', 'volkov1@mail.com', 'ДОГ006', '2024-01-06', 16, 'Улица Зловещая, Город Зла'),
('Михайлов', 'Михаил', 'Сергеевич', '72345678', 'mikhailov1@mail.com', 'ДОГ007', '2024-01-07', 17, 'Улица Цикличная, Город Гоночный'),
('Федоров', 'Федор', 'Михайлович', '899949623', 'fedorov1@mail.com', 'ДОГ008', '2024-01-08', 18, 'Улица Жирафа, Город Зоопарк'),
('Николаев', 'Николай', 'Федорович', '79994959319', 'nikolaev1@mail.com', 'ДОГ009', '2024-01-09', 19, 'Улица Птеродактеля, Город Томск'),
('Орлов', 'Олег', 'Николаевич', '892134214', 'orlov1@mail.com', 'ДОГ010', '2024-01-10', 20, 'Улица Святой-Марии, Город Кривой-рог');


INSERT INTO address_clients (clients_id, city, street, house, flat, floor) VALUES
(31, 'Город Томск', 'Улица Суворова', '10', '1', '1'),
(32, 'Город Космический', 'Улица Клименто', '20', '2', '2'),
(33, 'Город Великий', 'Улица Великого', '30', '3', '3'),
(34, 'Город Красоты', 'Улица Прекрасного', '40', '4', '4'),
(35, 'Город Чудес', 'Улица Чудесная', '50', '5', '5'),
(36, 'Город Зла', 'Улица Зловещая', '60', '6', '6'),
(37, 'Город Гоночный', 'Улица Цикличная', '70', '7', '7'),
(38, 'Город Зоопарк', 'Улица Жирафа', '80', '8', '8'),
(39, 'Город Томск', 'Улица Птеродактеля', '90', '9', '9'),
(40, 'Город Кривой-рог', 'Улица Святой-Марии', '100', '10', '10');

INSERT INTO incidents_reports (security_posts_and_orders_id, employees_id, report_date, description, status)
VALUES
(1, 21, '2025-02-01', 'Пожар на объекте, требуется эвакуация. Работники были проинформированы.', 'Новый'),
(2, 22, '2025-02-02', 'На посту была замечена подозрительная активность, необходимо проверить территорию.', 'В процессе'),
(3, 23, '2025-02-03', 'Неисправность оборудования на объекте, требуется замена датчиков.', 'Закрыт'),
(4, 24, '2025-02-04', 'Воришка был замечен на территории, вызвана полиция для расследования.', 'Новый'),
(5, 25, '2025-02-05', 'Обнаружен поврежденный забор, необходимо немедленно провести ремонт.', 'В процессе'),
(6, 26, '2025-02-06', 'Вышел из строя замок на главном входе, требуется замена.', 'Закрыт'),
(7, 27, '2025-02-07', 'Пропажа инвентаря с территории охраны, проводится проверка.', 'Новый'),
(8, 28, '2025-02-08', 'Нарушение графика работы охраны, требуется корректировка смен.', 'В процессе'),
(9, 29, '2025-02-09', 'Обнаружены подозрительные следы на территории объекта, требуется дополнительное обследование.', 'Новый'),
(10, 30, '2025-02-10', 'Пожарная безопасность не была соблюдена на одном из объектов, требуется проверка.', 'Закрыт');

INSERT INTO services (name, description, duration, price, status) VALUES
('Охрана территории', 'Ночной обход', '8', 100.00, 'активна'),
('Охрана VIP', 'Личная охрана', '12', 500.00, 'активна'),
('Видеонаблюдение', 'Удаленный мониторинг', '24', 150.00, 'активна'),
('Охрана мероприятий', 'Контроль толпы', '6', 200.00, 'активна'),
('Охрана банка', 'Вооруженные охранники', '24', 300.00, 'активна'),
('Охрана торгового центра', 'Охрана магазина', '12', 250.00, 'активна'),
('Охрана офиса', 'Безопасность офиса', '8', 180.00, 'активна'),
('Охрана склада', 'Охрана хранилища', '12', 220.00, 'активна'),
('Охрана стройки', 'Охрана стройплощадки', '24', 260.00, 'активна'),
('Охрана больницы', 'Охрана медучреждения', '12', 230.00, 'активна');


INSERT INTO orders (clients_id, services_id, orders_date, status, total_price) VALUES
(31, 1, '2024-02-01', 'завершен', 100.00),
(32, 2, '2024-02-02', 'в процессе', 500.00),
(33, 3, '2024-02-03', 'активен', 150.00),
(34, 4, '2024-02-04', 'активен', 200.00),
(35, 5, '2024-02-05', 'завершен', 300.00),
(36, 6, '2024-02-06', 'в процессе', 250.00),
(37, 7, '2024-02-07', 'активен', 180.00),
(38, 8, '2024-02-08', 'завершен', 220.00),
(39, 9, '2024-02-09', 'в процессе', 260.00),
(40, 10, '2024-02-10', 'активен', 230.00);


INSERT INTO invoices (orders_id, issue_date, due_date, total_amount, status) VALUES
(21, '2024-02-02', '2024-03-02', 100.00, 'оплачен'),
(22, '2024-02-03', '2024-03-03', 500.00, 'не оплачен'),
(23, '2024-02-04', '2024-03-04', 150.00, 'оплачен'),
(24, '2024-02-05', '2024-03-05', 200.00, 'не оплачен'),
(25, '2024-02-06', '2024-03-06', 300.00, 'оплачен'),
(26, '2024-02-07', '2024-03-07', 250.00, 'не оплачен'),
(27, '2024-02-08', '2024-03-08', 180.00, 'оплачен'),
(28, '2024-02-09', '2024-03-09', 220.00, 'не оплачен'),
(29, '2024-02-10', '2024-03-10', 260.00, 'оплачен'),
(30, '2024-02-11', '2024-03-11', 230.00, 'не оплачен');
select * from orders

INSERT INTO security_posts (adress, type, security_level, status) VALUES
('Город Томск, Улица Суворова', 'статичный', 'высокий', 'активен'),
('Город Космический, Улица Клименто', 'мобильный', 'средний', 'активен'),
('Город Великий, Улица Великого', 'статичный', 'низкий', 'активен'),
('Город Красоты, Улица Прекрасного', 'мобильный', 'высокий', 'активен'),
('Город Чудес, Улица Чудесная', 'статичный', 'средний', 'активен'),
('Город Зла, Улица Зловещая', 'мобильный', 'низкий', 'активен'),
('Город Гоночный, Улица Цикличная', 'статичный', 'высокий', 'активен'),
('Город Зоопарк, Улица Жирафа', 'мобильный', 'средний', 'активен'),
('Город Томск, Улица Птеродактеля', 'статичный', 'низкий', 'активен'),
('Город Кривой-рог, Улица Святой-Марии', 'мобильный', 'высокий', 'активен');


INSERT INTO security_posts_and_orders (orders_id, security_posts_id) VALUES
(21, 1),
(22, 2),
(23, 3),
(24, 4),
(25, 5),
(26, 6),
(27, 7),
(28, 8),
(29, 9),
(30, 10);


INSERT INTO employees (surname, name, patronymic, phone_number, email, hire_date, status, users_id) VALUES
('Сергеев', 'Сергей', 'Александрович', '898765434', 'sergeev@mail.com', '2023-01-01', 'активен', 11),
('Кузнецов', 'Александр', 'Сергеевич', '898765436', 'kuznetsov@mail.com', '2023-02-01', 'активен', 12),
('Фролов', 'Игорь', 'Александрович', '891762436', 'frolov@mail.com', '2023-03-01', 'активен', 13),
('Павлов', 'Денис', 'Сергеевич', '898762334', 'pavlov@mail.com', '2023-04-01', 'активен', 14),
('Гаврилов', 'Артем', 'Игорьевич', '898875434', 'gavrilov@mail.com', '2023-05-01', 'активен', 15),
('Титов', 'Олег', 'Артемович', '898765412', 'titov@mail.com', '2023-06-01', 'активен', 16),
('Борисов', 'Никита', 'Олегович', '898754434', 'borisov@mail.com', '2023-07-01', 'активен', 17),
('Егоров', 'Павел', 'Никитович', '898762334', 'egorov@mail.com', '2023-08-01', 'активен', 18),
('Зайцев', 'Андрей', 'Павлович', '8987689434', 'zaytsev@mail.com', '2023-09-01', 'активен', 19),
('Макаров', 'Василий', 'Андреевич', '891765434', 'makarov@mail.com', '2023-10-01', 'активен', 20);


INSERT INTO employees_assignments (orders_id, employees_id, assigned_date, shift_start, shift_end) VALUES
(21, 21, '2024-02-01', '08:00', '20:00'),
(22, 22, '2024-02-02', '09:00', '21:00'),
(23, 23, '2024-02-03', '10:00', '22:00'),
(24, 24, '2024-02-04', '11:00', '23:00'),
(25, 25, '2024-02-05', '12:00', '00:00'),
(26, 26, '2024-02-06', '13:00', '01:00'),
(27, 27, '2024-02-07', '14:00', '02:00'),
(28, 28, '2024-02-08', '15:00', '03:00'),
(29, 29, '2024-02-09', '16:00', '04:00'),
(30, 30, '2024-02-10', '17:00', '05:00');


INSERT INTO equipments (name, type, status, assigned_to) VALUES
('Рация', 'связь', 'в рабочем состоянии', 1),
('Камера', 'наблюдение', 'в рабочем состоянии', 2),
('Металлодетектор', 'безопасность', 'в рабочем состоянии', 3),
('Бронежилет', 'защита', 'в рабочем состоянии', 4),
('Сигнализация', 'охрана', 'в рабочем состоянии', 5),
('Фонарик', 'освещение', 'в рабочем состоянии', 6),
('Газовый баллончик', 'самозащита', 'в рабочем состоянии', 7),
('Наручники', 'контроль', 'в рабочем состоянии', 8),
('Ключи', 'контроль доступа', 'в рабочем состоянии', 9),
('Шлем', 'защита', 'в рабочем состоянии', 10);

INSERT INTO security_posts_and_equipments (security_posts_id, equipments_id) VALUES
(1, 1),
(2, 2),
(3, 3),
(4, 4),
(5, 5),
(6, 6),
(7, 7),
(8, 8),
(9, 9),
(10, 10);


INSERT INTO security_posts_and_services (employees_id, services_id) VALUES
(21, 1),
(22, 2),
(23, 3),
(24, 4),
(25, 5),
(26, 6),
(27, 7),
(28, 8),
(29, 9),
(30, 10);

-- Создание роли для Админа с правами подключения и создания баз данных
CREATE ROLE admin LOGIN PASSWORD 'password123';
GRANT CONNECT ON DATABASE user16 TO admin;
GRANT CREATE ON DATABASE user16 TO admin;

-- Создание роли для Менеджера с правами подключения
CREATE ROLE manager LOGIN PASSWORD 'password123';
GRANT CONNECT ON DATABASE user16 TO manager;

-- Создание роли для Клиента с правами подключения
CREATE ROLE client LOGIN PASSWORD 'password123';
GRANT CONNECT ON DATABASE user16 TO client;

-- Представление для Админа: Просмотр всех заказов
CREATE VIEW admin_orders_view AS
SELECT orders_id, clients_id, services_id, orders_date, status, total_price
FROM orders;
GRANT SELECT ON admin_orders_view TO admin;

-- Представление для Менеджера: Просмотр отчетов по заказам
CREATE VIEW manager_reports_view AS
SELECT o.orders_id, o.clients_id, o.services_id, o.orders_date, o.status, o.total_price, i.status AS payment_status
FROM orders o
LEFT JOIN invoices i ON o.orders_id = i.orders_id;
GRANT SELECT ON manager_reports_view TO manager;

-- Представление для Клиента: Просмотр своих заказов с дополнительной информацией
CREATE VIEW client_orders_view AS
SELECT o.orders_id, o.services_id, s.name AS service_name, e.name AS employee_name,
       sp.adress AS execution_address, o.orders_date, o.status, o.total_price
FROM orders o
JOIN clients c ON o.clients_id = c.clients_id
JOIN users u ON c.users_id = u.users_id
JOIN services s ON o.services_id = s.services_id
LEFT JOIN employees e ON o.employees_id = e.employees_id
LEFT JOIN security_posts_and_orders spo ON o.orders_id = spo.orders_id
LEFT JOIN security_posts sp ON spo.security_posts_id = sp.security_posts_id
WHERE u.username = current_user;


-- Представление для Клиента: Просмотр своих счетов
CREATE VIEW client_invoice_view AS
SELECT i.invoices_id, i.orders_id, i.issue_date, i.due_date, i.total_amount, i.status
FROM invoices i
WHERE i.orders_id IN (
    SELECT o.orders_id FROM orders o
    JOIN clients c ON o.clients_id = c.clients_id
    JOIN users u ON c.users_id = u.users_id
    WHERE u.username = current_user
);
GRANT SELECT ON client_invoice_view TO client;

-- Представление для Сотрудника: Просмотр услуг, клиентов и статуса оплаты
CREATE VIEW employee_orders_view AS
SELECT o.orders_id, s.name AS service_name,
       CONCAT(c.surname, ' ', c.name, ' ', COALESCE(c.patronymic, '')) AS client_name,
       i.status AS payment_status
FROM orders o
JOIN services s ON o.services_id = s.services_id
JOIN clients c ON o.clients_id = c.clients_id
LEFT JOIN invoices i ON o.orders_id = i.orders_id
JOIN employees e ON o.employees_id = e.employees_id
JOIN users u ON e.users_id = u.users_id
WHERE u.username = current_user;


-- Представление для Сотрудника: Просмотр экипировки
CREATE VIEW employee_equipment_view AS
SELECT e.equipments_id, e.name AS equipment_name, e.status
FROM equipments e
JOIN security_posts_and_equipments spe ON e.equipments_id = spe.equipments_id
JOIN security_posts_and_orders spo ON spe.security_posts_id = spo.security_posts_id
JOIN orders o ON spo.orders_id = o.orders_id
JOIN employees emp ON o.employees_id = emp.employees_id
JOIN users u ON emp.users_id = u.users_id
WHERE u.username = current_user;


-- Представление для Пользователя: Просмотр всех доступных услуг
CREATE VIEW user_services_view AS
SELECT services_id, name, description, price
FROM services;
GRANT SELECT ON user_services_view TO public;

-- Админ: для получения всех заказов
SELECT * FROM admin_orders_view;

-- Менеджер: для получения отчетов по заказам
SELECT * FROM manager_reports_view;

-- Клиент: для получения своих заказов
SELECT * FROM client_orders_view;

-- Клиент: для получения своих счетов
SELECT * FROM client_invoice_view;

-- Сотрудник: для получения информации о заказах
SELECT * FROM employee_orders_view;

-- Сотрудник: для просмотра своей экипировки
SELECT * FROM employee_equipment_view;

-- Пользователь: для просмотра всех услуг
SELECT * FROM user_services_view;
