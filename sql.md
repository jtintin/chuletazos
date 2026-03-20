# --- BASES DE DATOS DE 0 A PRO (MYSQL, POSTGRESQL, MONGODB, SQLSERVER) ---

# --- MYSQL ---
# Puerto por defecto: 3306
mysql -u root -p                          -- entrar al servidor MySQL
CREATE DATABASE tienda;                   -- crear base de datos llamada tienda
USE tienda;                               -- seleccionar la base de datos

CREATE TABLE clientes (                   -- crear tabla clientes
  id INT PRIMARY KEY AUTO_INCREMENT,      -- clave primaria autoincremental
  nombre VARCHAR(50),                     -- nombre del cliente
  email VARCHAR(100) UNIQUE               -- email único
);

CREATE TABLE pedidos (                    -- crear tabla pedidos relacionada
  id INT PRIMARY KEY AUTO_INCREMENT,      -- clave primaria autoincremental
  cliente_id INT,                         -- referencia al cliente
  fecha DATE,                             -- fecha del pedido
  total DECIMAL(10,2),                    -- monto total
  FOREIGN KEY (cliente_id) REFERENCES clientes(id) -- relación con clientes
);

INSERT INTO clientes (nombre,email) VALUES ('Juan','juan@mail.com'),('Ana','ana@mail.com'); -- insertar clientes
INSERT INTO pedidos (cliente_id,fecha,total) VALUES (1,'2026-03-01',120.50),(2,'2026-03-05',200.00); -- insertar pedidos

SELECT c.nombre, SUM(p.total) AS gasto_total   -- JOIN + SUM + GROUP BY + HAVING
FROM clientes c JOIN pedidos p ON c.id = p.cliente_id
GROUP BY c.nombre HAVING SUM(p.total) > 100;   -- filtrar clientes con gasto > 100

CREATE VIEW vista_clientes_pedidos AS          -- crear vista para reutilizar consulta
SELECT c.nombre, p.total FROM clientes c JOIN pedidos p ON c.id=p.cliente_id;

DELIMITER //
CREATE FUNCTION total_pedidos(id_cliente INT) RETURNS DECIMAL(10,2)  -- función que devuelve gasto total de un cliente
BEGIN
  DECLARE total DECIMAL(10,2);
  SELECT SUM(total) INTO total FROM pedidos WHERE cliente_id=id_cliente;
  RETURN IFNULL(total,0);
END //
DELIMITER ;

DELIMITER //
CREATE PROCEDURE pedidos_mayores(IN monto DECIMAL(10,2))             -- procedimiento que lista pedidos mayores a monto
BEGIN
  SELECT * FROM pedidos WHERE total > monto;
END //
DELIMITER ;

mysqldump -u root -p tienda > backup.sql        -- backup de la base tienda
mysql -u root -p tienda < backup.sql            -- restaurar backup

CREATE USER 'juan'@'localhost' IDENTIFIED BY 'clave'; -- crear usuario juan
GRANT ALL PRIVILEGES ON tienda.* TO 'juan'@'localhost'; -- asignar permisos completos
FLUSH PRIVILEGES;                                -- aplicar cambios

# --- POSTGRESQL ---
# Puerto por defecto: 5432
psql -U postgres                               -- entrar a PostgreSQL
CREATE DATABASE tienda;                        -- crear base
\c tienda                                      -- conectar a base

CREATE TABLE clientes (id SERIAL PRIMARY KEY, nombre VARCHAR(50), email VARCHAR(100) UNIQUE); -- tabla clientes
CREATE TABLE pedidos (id SERIAL PRIMARY KEY, cliente_id INT REFERENCES clientes(id), fecha DATE, total NUMERIC(10,2)); -- tabla pedidos

INSERT INTO clientes (nombre,email) VALUES ('Luis','luis@mail.com'),('Maria','maria@mail.com'); -- insertar clientes
INSERT INTO pedidos (cliente_id,fecha,total) VALUES (1,'2026-03-02',150.00),(2,'2026-03-06',300.00); -- insertar pedidos

SELECT c.nombre, SUM(p.total) AS gasto_total   -- JOIN + SUM + HAVING
FROM clientes c JOIN pedidos p ON c.id=p.cliente_id
GROUP BY c.nombre HAVING SUM(p.total)>100;     -- filtrar clientes con gasto > 100

CREATE VIEW vista_clientes_pedidos AS          -- vista
SELECT c.nombre,p.total FROM clientes c JOIN pedidos p ON c.id=p.cliente_id;

CREATE OR REPLACE FUNCTION total_pedidos(id_cliente INT) RETURNS NUMERIC AS $$ -- función
  SELECT COALESCE(SUM(total),0) FROM pedidos WHERE cliente_id=id_cliente;
$$ LANGUAGE SQL;

CREATE OR REPLACE PROCEDURE pedidos_mayores(monto NUMERIC) LANGUAGE SQL AS $$  -- procedimiento
  SELECT * FROM pedidos WHERE total > monto;
$$;

pg_dump -U postgres tienda > backup.sql        -- backup
psql -U postgres tienda < backup.sql           -- restore

CREATE USER juan WITH PASSWORD 'clave';        -- crear usuario
GRANT ALL PRIVILEGES ON DATABASE tienda TO juan; -- permisos

# --- MONGODB ---
# Puerto por defecto: 27017
mongosh                                        -- entrar a MongoDB
use tienda                                     -- crear/usar base

db.clientes.insertMany([                       -- insertar clientes
  {_id:1,nombre:"Carlos",email:"carlos@mail.com"},
  {_id:2,nombre:"Sofia",email:"sofia@mail.com"}
]);

db.pedidos.insertMany([                        -- insertar pedidos
  {cliente_id:1,fecha:ISODate("2026-03-03"),total:180.00},
  {cliente_id:2,fecha:ISODate("2026-03-07"),total:250.00}
]);

db.clientes.aggregate([                        -- JOIN equivalente con $lookup
  {$lookup:{from:"pedidos",localField:"_id",foreignField:"cliente_id",as:"pedidos_cliente"}},
  {$project:{nombre:1,pedidos_cliente:1}}
]);

db.pedidos.aggregate([                         -- SUM + HAVING equivalente con $group + $match
  {$group:{_id:"$cliente_id",gasto_total:{$sum:"$total"}}},
  {$match:{gasto_total:{$gt:100}}}
]);

mongodump --db tienda --out /ruta/backup       -- backup
mongorestore --db tienda /ruta/backup/tienda   -- restore

use admin
db.createUser({user:"juan",pwd:"clave",roles:[{role:"readWrite",db:"tienda"}]}); -- permisos

# --- SQL SERVER ---
# Puerto por defecto: 1433
sqlcmd -S localhost -U sa -P clave             -- entrar a SQL Server
CREATE DATABASE tienda;                        -- crear base
USE tienda;                                    -- usar base

CREATE TABLE clientes (id INT PRIMARY KEY IDENTITY, nombre NVARCHAR(50), email NVARCHAR(100) UNIQUE); -- tabla clientes
CREATE TABLE pedidos (id INT PRIMARY KEY IDENTITY, cliente_id INT FOREIGN KEY REFERENCES clientes(id), fecha DATE, total DECIMAL(10,2)); -- tabla pedidos

INSERT INTO clientes (nombre,email) VALUES ('Pedro','pedro@mail.com'),('Laura','laura@mail.com'); -- insertar clientes
INSERT INTO pedidos (cliente_id,fecha,total) VALUES (1,'2026-03-04',220.00),(2,'2026-03-08',90.00); -- insertar pedidos

SELECT c.nombre, SUM(p.total) AS gasto_total   -- JOIN + SUM + HAVING
FROM clientes c JOIN pedidos p ON c.id=p.cliente_id
GROUP BY c.nombre HAVING SUM(p.total)>100;     -- filtrar clientes con gasto > 100

CREATE VIEW vista_clientes_pedidos AS          -- vista
SELECT c.nombre,p.total FROM clientes c JOIN pedidos p ON c.id=p.cliente_id;

CREATE FUNCTION total_pedidos(@id_cliente INT) RETURNS DECIMAL(10,2) AS  -- función
BEGIN
  DECLARE @total DECIMAL(10,2);
  SELECT @total=SUM(total) FROM pedidos WHERE cliente_id=@id_cliente;
  RETURN ISNULL(@total,0);
END;

BACKUP DATABASE tienda TO DISK='C:\backup\tienda.bak'; -- backup
RESTORE DATABASE tienda FROM DISK='C:\backup\tienda.bak'; -- restore

CREATE LOGIN juan WITH PASSWORD='clave';       -- crear login
CREATE USER juan FOR LOGIN juan;               -- crear usuario
GRANT SELECT,INSERT,UPDATE,DELETE ON clientes TO juan; -- permisos

