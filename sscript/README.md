Archivo texto que contiene la base de datos relacional y uan serie de sentencias DDL y DML

📌 Flujo para ejecutar el script SQL del proyecto

Este proyecto incluye la creación, configuración y carga inicial de una base de datos para la gestión de una banda de metal.
A continuación se detalla el flujo recomendado para ejecutar correctamente todas las instrucciones DDL (definición de estructura) y DML (manipulación de datos).

🚀 1. Crear la base de datos

El primer paso consiste en generar la base donde se almacenarán todas las tablas del proyecto:

CREATE DATABASE banda_metal_db;
USE banda_metal_db;

🏗️ 2. Crear las tablas (DDL)

Se deben ejecutar las sentencias en el siguiente orden, ya que algunas tablas dependen de otras por claves foráneas:

1. Tabla banda

(Base principal para las relaciones)

CREATE TABLE banda (
    id INT PRIMARY KEY AUTO_INCREMENT NOT NULL,
    nombre VARCHAR(100) NOT NULL,
    descripcion TEXT,
    anio_formacion INT
);

2. Tabla miembros

(Depende de banda mediante banda_id)

CREATE TABLE miembros (
    id INT PRIMARY KEY AUTO_INCREMENT NOT NULL,
    nombre VARCHAR(100),
    instrumento VARCHAR(100),
    fecha_ingreso DATE,
    banda_id INT,
    FOREIGN KEY (banda_id) REFERENCES banda(id)
);

3. Tabla fotos
CREATE TABLE fotos (
    id INT PRIMARY KEY AUTO_INCREMENT NOT NULL,
    url VARCHAR(255),
    descripcion VARCHAR(255),
    banda_id INT,
    FOREIGN KEY (banda_id) REFERENCES banda(id)
);

4. Tabla videos
CREATE TABLE videos (
    id INT PRIMARY KEY AUTO_INCREMENT NOT NULL,
    titulo VARCHAR(255),
    url VARCHAR(255),
    banda_id INT,
    FOREIGN KEY (banda_id) REFERENCES banda(id)
);

5. Tabla mensajes_contacto
CREATE TABLE mensajes_contacto (
    id INT PRIMARY KEY AUTO_INCREMENT NOT NULL,
    nombre VARCHAR(100) NOT NULL,
    email VARCHAR(150) NOT NULL,
    asunto VARCHAR(200),
    mensaje TEXT NOT NULL,
    fecha DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    leido TINYINT(1) NOT NULL DEFAULT 0
);

📥 3. Insertar datos iniciales (DML – INSERT)
Insertar bandas
INSERT INTO banda (nombre, descripcion, anio_formacion)
VALUES ('Kolosal Remaint', 'Banda de metal chilena con influencias thrash y groove.', 2022);

INSERT INTO banda (nombre, descripcion, anio_formacion)
VALUES ('Metal Fury', 'Banda de metal chilena con estilo heavy y thrash.', 2015);

Insertar miembros
INSERT INTO miembros (nombre, instrumento, fecha_ingreso, banda_id)
VALUES
('Martin Castro', 'Voz / Guitarra', '2022-03-10', 1),
('Carlos Soto', 'Batería', '2022-05-22', 1),
('Javier Torres', 'Bajo', '2022-08-15', 1);

INSERT INTO miembros (nombre, instrumento, fecha_ingreso, banda_id)
VALUES 
('Brian Pradenas', 'Voz', '2016-01-10', 2),
('Carlos Herrera', 'Guitarra', '2017-05-21', 2),
('Matías Soto', 'Batería', '2018-09-12', 2);

Insertar mensaje de contacto
INSERT INTO mensajes_contacto (nombre, email, asunto, mensaje)
VALUES (
    'Andrea López',
    'andrea.lopez@gmail.com',
    'Contratación de la banda',
    'Hola, quisiera saber disponibilidad para un evento en diciembre.'
);

✏️ 4. Modificaciones de datos (DML – UPDATE / DELETE)
Actualizar descripción de banda
UPDATE banda
SET descripcion = 'Banda chilena de metal melódico con influencias modernas.'
WHERE id = 2;

Cambiar instrumento de un miembro
UPDATE miembros
SET instrumento = 'Guitarra Líder'
WHERE id = 1;

❌ 5. Eliminaciones (DELETE)
Eliminar un miembro en específico
DELETE FROM miembros
WHERE nombre = 'Carlos Herrera' AND banda_id = 2;

Eliminar todos los miembros de una banda
DELETE m FROM miembros m
JOIN banda b ON m.banda_id = b.id
WHERE b.nombre = 'Metal Fury';

🔍 6. Consultas SELECT

Aquí se realizan pruebas y visualizaciones de la información almacenada:

Ver todas las bandas
SELECT * FROM banda;

Mensajes NO leídos
SELECT *
FROM mensajes_contacto
WHERE leido = 0
ORDER BY fecha DESC;

Obtener miembros + nombre de la banda
SELECT m.nombre, m.instrumento, b.nombre AS banda
FROM miembros m
JOIN banda b ON m.banda_id = b.id;

Bandas con o sin fotos
SELECT b.nombre, f.url
FROM banda b
LEFT JOIN fotos f ON f.banda_id = b.id;

🧩 7. Orden recomendado para ejecutar todo

Crear la base de datos

Crear todas las tablas (DDL)

Insertar datos iniciales (DML – INSERT)

Realizar actualizaciones y eliminaciones (UPDATE / DELETE)

Ejecutar consultas de verificación (SELECT)
